# Nikto Process Execution Sample

In this example we use a pod deployment designed to trigger a custom policy in Red Hat Advanced Cluster Security (ACS). The policy is configured to detect the execution of the Nikto tool during the runtime phase of a container within a pod in the OpenShift cluster.

### Additional Resources
- [Nikto](https://github.com/sullo/nikto)

## Pod Specification

The pod, named `alpine-test-pod`, is deployed in the `turbra-hax` namespace. It uses an Alpine Linux image configured to run Nikto.

### Deployment YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: alpine-test-pod
  namespace: turbra-hax
spec:
  containers:
  - name: alpine-nikto
    image: alpine/nikto
    command: ["sh", "-c", "echo Hello Friend! && nikto.pl && sleep 3600"]
```

## ACS Policy Configuration

- **Lifecycle Stage**: The policy is set to trigger at the `Runtime` stage.
- **Event Source**: The policy is focused on the `Deployment` event source.
- **Response and Integration**: Although the policy response is set to `Inform`, we have enabled a custom webhook notifier within ACS. This notifier is configured to trigger a specific GitLab CI/CD pipeline job upon policy violation detection. Consequently, while ACS reports the event without interfering with the pod's operation, the webhook integration initiates an automated response via the GitLab pipeline.

## Purpose

This deployment is intended for testing and demonstrating the ACS policy's ability to detect specific processes (like Nikto) running within containers in a Kubernetes/OpenShift environment.

## Usage

Deploy this pod in an OpenShift cluster where ACS is monitoring for policy violations. Once the pod is running, ACS should detect the execution of `nikto.pl` as defined in the policy.
```bash
oc apply -f bad_pod.yaml
```

## Testing and Validation

- Deploy the pod and monitor ACS for alerts related to policy violations.
- Ensure ACS is correctly configured to detect the specified conditions and respond as expected.
- Ensure GitLab job is executed and completes without errors.

```bash
$ ansible-playbook ./samples/bad_pod/main.yml
[WARNING]: provided hosts list is empty, only localhost is available. Note that
the implicit localhost does not match 'all'
PLAY [localhost] ***************************************************************
TASK [Check if the TRIGGER_PAYLOAD file exists] ********************************
ok: [localhost]
TASK [Fail if TRIGGER_PAYLOAD file does not exist] *****************************
skipping: [localhost]
TASK [Read the JSON payload from the file] *************************************
ok: [localhost]
TASK [Decode JSON payload] *****************************************************
ok: [localhost]
TASK [Set facts from JSON payload] *********************************************
ok: [localhost]
TASK [Fail if namespace is not found in the payload] ***************************
skipping: [localhost]
TASK [Generate network policy from template] ***********************************
ok: [localhost]
TASK [Display Policy Severity] *************************************************
ok: [localhost] => {
    "msg": "Policy Severity: CRITICAL_SEVERITY"
}
TASK [Display Namespace] *******************************************************
ok: [localhost] => {
    "msg": "Namespace: turbra-hax"
}
TASK [Display Container Name] **************************************************
ok: [localhost] => {
    "msg": "Container Name: alpine-nikto"
}
TASK [Display Policy Name] *****************************************************
ok: [localhost] => {
    "msg": "Policy Name: turbra_hax_nikto"
}
TASK [Display Policy Description] **********************************************
ok: [localhost] => {
    "msg": "Policy Description: Detect the execution of Nikto"
}
TASK [Display Policy Categories] ***********************************************
ok: [localhost] => {
    "msg": "Policy Categories: Anomalous Activity"
}
TASK [Display Policy Lifecycle] ************************************************
ok: [localhost] => {
    "msg": "Policy Lifecycle: RUNTIME"
}
TASK [Display Violation Date/Time] *********************************************
ok: [localhost] => {
    "msg": "Violation Date/Time: 2024-02-12T15:36:23.034933797Z"
}
TASK [Display the content of the generated network policy] *********************
changed: [localhost]
TASK [Show the content of the network policy file] *****************************
ok: [localhost] => {
    "cat_output.stdout": "apiVersion: networking.k8s.io/v1\nkind: NetworkPolicy\nmetadata:\n  name: deny-all\n  namespace: \"turbra-hax\"\nspec:\n  podSelector: {}\n  policyTypes:\n    - Ingress\n    - Egress"
}
TASK [Apply the network policy to the namespace] *******************************
ok: [localhost]
PLAY RECAP *********************************************************************
localhost                  : ok=16   changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
Cleaning up project directory and file based variables
00:00
Job succeeded
```
