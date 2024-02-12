# Setup Guide for GitLab Project Triggers

This README provides instructions on setting up and configuring GitLab project triggers. These triggers are essential for automating tasks in response to specific events, such as commits, merges, or external triggers from tools like Red Hat Advanced Cluster Security (ACS).

## Prerequisites

- A GitLab account with appropriate permissions to create and manage project triggers.
- A project in GitLab where the CI/CD pipeline is configured.

## Step-by-Step Guide

### 1. Creating a Trigger Token

1. **Navigate to Your Project**: Go to your project in GitLab.
2. **Access CI/CD Settings**: In your project, go to `Settings` > `CI / CD`.
3. **Expand pipeline trigger tokens section**: Scroll down to the `Triggers` section and click to expand it.
4. **Add Trigger**: Click the `Add trigger` button.
5. **Copy the Trigger Token**: Once the trigger is created, copy the token. This token will be used to authenticate the trigger requests.

### 2. Configuring the Trigger

1. **Prepare the Trigger URL**: Construct the trigger URL using the format: 
   ```
   http://<gitlab-instance>/api/v4/projects/<project-id>/trigger/pipeline
   ```
   Replace `<gitlab-instance>` with your GitLab instance URL and `<project-id>` with your project's ID.

2. **Include the Token and Reference**: Append the following parameters to the URL:
   ```
   ?token=<trigger-token>&ref=<branch-name>
   ```
   Replace `<trigger-token>` with the trigger token you copied earlier and `<branch-name>` with the branch name you want to trigger the pipeline on.

3. **Example of a Complete Trigger URL**:
   ```
   http://gitlab.example.com/api/v4/projects/123/trigger/pipeline?token=abc123def456&ref=main
   ```

## Additional Resources

- [GitLab CI/CD Pipeline Configuration Reference](https://docs.gitlab.com/ee/ci/yaml/)
- [GitLab Trigger API Documentation](https://docs.gitlab.com/ee/ci/triggers/)
