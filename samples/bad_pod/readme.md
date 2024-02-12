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
