# Applying ACS Generic Webhook Notifier to Existing Policies

This README outlines the steps to apply a newly created generic webhook notifier to existing policies in Red Hat Advanced Cluster Security (ACS). This integration enables ACS to send notifications to external systems, such as GitLab CI/CD pipelines, in response to policy violations.

## Prerequisites

- An existing ACS installation with administrative access.
- A configured generic webhook notifier in ACS.
- Existing policies within ACS for which you want to enable webhook notifications.

## Step-by-Step Guide

## 1. Accessing ACS

1. **Log into ACS**: Access the ACS interface using your administrator credentials.

2. **Navigate to Policy Management**: Go to the `Platform Configuration` section and select `Policy Management`.

### 2. Selecting a Policy

1. **Choose a Policy**: From the list of policies, select the policy you want to attach the webhook notifier to. You can use search and filter options to find specific policies.

### 3. Configuring the Policy

1. **Edit the Policy**: Click on the selected policy to open its details. Then click on `Actions` and select `Edit policy` to modify the policy.
2. **Attach Notifiers**: On the right side of the page, locate the `Attach Notifiers` section.
3. **Add Webhook Notifier**: Select the previously configured generic webhook notifier from the list of available notifiers.
4. **Save the Policy**: After adding the webhook notifier, save the changes to the policy by clicking `Review policy` and then selecting `Save`.

## Additional Resources

- [ACS Documentation](https://docs.openshift.com/acs/4.3/welcome/index.html)
- [Managing Security Policies](https://docs.openshift.com/acs/4.3/operating/manage-security-policies.html)
