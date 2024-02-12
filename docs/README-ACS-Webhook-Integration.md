# Setting Up ACS Generic Webhook Integration

This README provides instructions on how to set up a generic webhook integration in Red Hat Advanced Cluster Security (ACS). This integration enables ACS to trigger external services, like GitLab CI/CD pipelines, in response to specific events, such as policy violations.

## Prerequisites

- An ACS (Red Hat Advanced Cluster Security) installation with administrative access.
- A target external service (e.g., GitLab CI/CD) that can accept incoming webhook requests.

## Step-by-Step Guide

### 1. Accessing ACS

1. **Log into ACS**: Access the ACS interface using your administrator credentials.
2. **Navigate to Integrations**: Go to the `Platform Configuration` section and select `Integrations`.

### 2. Configuring the Webhook

1. **Add New Integration**: Click on `Add Integration` in the top right corner of the `Integrations` page.
2. **Configuring the Webhook**: Scroll down to `Notifier Integrations` and select `StackRox - Generic Webhook`

1. **Add New Integration**: Click on `New Integration` in the top right corner of the Integrations page.
3. **Configure Webhook Settings**:
   - **Name**: Give a meaningful name to your webhook integration.
   - **Endpoint URL**: Enter the URL of the external service you want to trigger. This is typically the URL provided by the service for receiving webhook requests.
   - **Authentication (if required)**: If the endpoint requires authentication, provide the necessary credentials or token.

### 3. Specifying the Trigger Events

1. **Select Events**: Choose the events that should trigger the webhook. For integrating with GitLab CI/CD, you might select events related to policy violations.
2. **Test the Configuration**: Use the provided option to test the webhook. This ensures that ACS can successfully communicate with the external service.

### 4. Saving the Integration

- After configuring the settings and testing the webhook, save the integration. ACS will now send webhook notifications to the specified endpoint when the selected events occur.

## Usage

- ACS will automatically trigger the configured webhook upon the occurrence of the selected events.
- Ensure the external service (e.g., GitLab CI/CD pipeline) is correctly set up to handle incoming webhook requests from ACS.

## Additional Resources

- [ACS Documentation](https://docs.openshift.com/acs/4.3/welcome/index.html)
- [Webhook Integration Best Practices](https://docs.openshift.com/acs/4.3/integration/integrate-using-generic-webhooks.html)
