# CloudBots

# Overview

## What are D9 cloudbots?

Cloud-Bots is an automatic remediation solution for AWS built on top of Dome9's Continuous Compliance capabilities

## Why and when would I need it?
What does it do?

If you have configured Dome9 Continuous Compliance policies for your cloud account, the Dome9 Compliance Engine continuously scans  your cloud account (AWS,Azure,GCP) for policy violations, and issues alerts and report for any issues that are found.

CloudBots provide automatic-remediation for issues, in which you configure your cloud account to  take specifiic remedial actions, using cloudbots, in response to specific compliance violations. The actions use Dome9 cloudbots, which typically are scripts, which perform actions on your cloud account.


This approach could reduce the load on security operators and significantly reduce the time to resolve security issues.

## How does it work?

Dome9 continuously  scans your cloud accounts (using compliance policies)  and sends findings for  failed rules to AWS SNS.

To add a remediation step, the rule is modified to include  a "remediation flag" in the compliance section so that the SNS event is tagged with what we want to do.
Each remediation bot that is tagged correlates to a file in the bots folder of the remediation function.
Lambda reads the message tags and looks for a tag that matches AUTO:
If any of those AUTO tags match a remediation that we have built out, it'll call that bot.
All of the methods are sending their events to an array called text_output. Once the function is finished working, this array is turned into a string and posted to SNSץ

## Deployment modes

You can deploy the cloudbots in a single AWS account, or across multiple accounts.
The default mode is 'single' account. Follow the onboarding procedure (below) for this. For multiple accounts, additional steps are required.

# Onboarding

## Setup your AWS account(s) for cloudbots
Follow these steps for each region.
1. Select the region (deployment links), click  [<img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=dome9CloudBots&templateURL=https://s3.amazonaws.com/dome9cftemplatesuseast1/cloudbots_cftemplate.yaml). 
This will launch a script in the AWS CloudFormation console, for your AWS account, to setup cloud-bots in the selected region.
1. In the **Select Template**, click **Next** (no need to make a selection)
1. In the **Parameters** section select the deployment mode, *single* or *multi* mode. Enter an email address for SNS notifications in the EmailAddress field (optional),then click **Next**
1. In the **Options** page, click **Next** (no need to make any selections)
1. In the **Review** page, select the options:

``` I acknowledge that AWS CloudFormation might create IAM resources```

``` I acknowledge that AWS CloudFormation might create IAM resources with custom names.```

6. Click **Create Change Set**, then click **Execute**. The stack will be created in your AWS account. It will appear as *dome9CloudBots* in the list of stacks in the CloudFormation console.
1. Select the stack, and then select the **Outputs** section.
1. Copy the two ARNs there, *OutputTopicARN* and *InputTopicARN* and save them; they will be used to setup the SNS to trigger the bots, and the output reporting channel.

## Onboarding for Multi mode
Follow these steps to setup the cloudbots to work across a number of accounts.

## Setup your Dome9 account
Follow these steps in your Dome9 account to tag the compliance rules & bundles to use bots as a remediation step.


# Prerequisites

# Use-case example
