---
type: Web Page
title: Connect to your cloud - Polars user guide
resource: https://docs.pola.rs/polars-cloud/connect-cloud
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Connect to your cloud

Polars Cloud requires connection to your cloud environment to execute queries. After account registration, a guided onboarding flow walks you through creating an organization, selecting a deployment stack, and connecting your first workspace.

## Registration onboarding

After creating your account, a three-step flow completes the initial setup:

- **Create your account**: Already done.
- **Create your organization**: name the organization that manages workspaces and team access.
- **Select your stack**: choose a deployment type for your first workspace.

Two deployment types are available:

- **AWS**: workloads run on AWS-managed services, deployed via a CloudFormation template.
- **Kubernetes**: workloads run on a Kubernetes cluster (EKS, GKE, AKS, or on-premises), deployed via Helm.

For Kubernetes setup, see the EKS, GKE, or AKS guides. The rest of this page covers the AWS path.

Naming your workspace

If you're unsure, you can use a temporary name. You can change the workspace name later under the workspace settings.

Clicking **Create default workspace** completes registration and opens the dashboard with a guided
setup checklist.

## Workspace setup

When you first access the dashboard after registration, a five-step setup checklist guides you through connecting your workspace.

The checklist steps are:

- Create workspace
- Link your AWS account
- Deploy CloudFormation stack
- Invite members
- Run your first query

## AWS infrastructure deployment

The **Link your AWS account** step provides three options depending on your AWS access:

- **Deploy to AWS**: opens the CloudFormation quick-create URL directly in your browser.
- **Deploy to AWS in an existing VPC**: uses your existing VPC rather than creating a new one.
- **Copy the setup link**: share the deployment URL with your operations team or AWS administrator.

CloudFormation permissions

Users without CloudFormation deployment permissions can easily share the deployment URL with their operations team or AWS administrators.

The CloudFormation quick-create page is pre-filled with the template URL and a stack name derived from your workspace name. You don't have to change anything.

Before clicking **Create stack**, scroll to the bottom of the page and check the acknowledgment that
the template creates IAM resources.

For detailed information about the AWS resources and architecture, see the AWS Infrastructure page.

Back in the Polars Cloud dashboard, the **Deploy CloudFormation stack** step tracks progress through
five stages: Network → Security → Policies → Deploying → Connected.

Stack deployment typically completes within 5 minutes.

## Invite members

After the stack is deployed, you can invite team members to your workspace by email.

One-time workspace setup

Cloud environment connection is required once per workspace. Team members invited to connected workspaces can immediately execute remote queries without additional setup.

This step is optional and you can click **skip** to continue. Members can be added later from the
Team page.

## Run your first query

The final setup step provides a sample query to verify your workspace is connected and ready.

The snippet uses `uv` for a self-contained run. If you don't have it yet, you can follow the
provided link on the dashboard to install it. On first execution, a browser window opens to
authenticate. After connecting your session to Polars Cloud, credentials are cached locally for the
duration of your session.

Once the query completes successfully, the setup confirms your workspace is fully configured.

## Start your next project

With your workspace connected to AWS, you can execute Polars queries remotely. Consider these next actions:

- Learn how to run queries remotely and get the most out of Polars Cloud
- Profile your queries to understand and optimize performance
- Learn about compute context configuration for performance optimization
- Invite team members to your connected workspace to collaborate on your next project.

# Citations

1. Source page: https://docs.pola.rs/polars-cloud/connect-cloud
