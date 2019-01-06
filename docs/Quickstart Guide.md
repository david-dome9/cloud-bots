# Quickstart Guide

* [Setup Steps](#setup-steps)
  * [Select deployment mode](#decide-on-deployment-mode)
  * [Outside of Dome9 Easy mode](#outside-of-dome9)
  * [Outside of Dome9](#outside-of-dome9)
  * [Within Dome9](#in-dome9)
* [Sample Setup Example](#sample-setup-example)

## Outside of Dome9

You can deploy this stack in us-east-1 using the link below. **To deploy the stack in another region, go to the "Deployment Links" tab.**

us-east-1  
[<img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=dome9CloudBots&templateURL=https://s3.amazonaws.com/dome9cftemplatesuseast1/cloudbots_cftemplate.yaml)

<br>

**Click the link and then click Next** 

In the parameters section choose whether to deploy in single or multi-account mode.

If you would like to set up an SNS subscriber for the output topic (recommended) then put an email in the EmailAddress field. 

**Click Next > Next**

**On the 4th page, check the 2 boxes that allow this template to create IAM resources with custom names** (This is for the role that is created for Lambda to perform the bots)

**Next, click on the 'Create Change Set'** at the bottom of the page. Then click 'Execute'.

From here, the stack will deploy. If there are no errors, go to the 'Outputs' tab and grab the two ARNs that were output. You'll need them later. 

**If you set up an SNS Subscriber**
Go to your email and confirm the subscription. SNS seems to have some odd behaviors if you leave the subscription for a while before confirming. 

<br>
<br>

### If you're running in multi-account mode
By default, the cross account roles will all need to be named "Dome9CloudBots". If you want a different name, add a new variable to the dome9AutoRemediations lambda function called "CROSS_ACCOUNT_ROLE_NAME" and set the value to the new name for the role.


#### Set up cross account roles for EACH account that will be remediated

```bash
cd cross_account_role_configs
```

Role creation needs to be done using something other than CloudFormation because CFTs don't output consistent role names.

#### Update trust_policy.json with the account ID in which the main function will live

#### There is a small bash script in this directory that you can run (create_role.sh) to create these roles. 
```bash 
./create_role.sh <aws profile>
```

## In Dome9

### Create a bundle to use for auto remediation. 
It's recommended but not required to break remediation bots into their own bundles. 
There is a sample bundle (sample_bundle.json) that can be used as a starting point.
The rule in the sample bundle will remove rules from the default security group if the SG is empty. 

### For all rules that you want to add remediation to, add the remediation tag to the "Compliance Section" of the rule. 

All available remediation bots are in the bots folder. 

#### Tag Syntax: AUTO: <bot_name> <params>
    Ex: AUTO: ec2_stop_instance

### Test this compliance bundle
Make sure you're getting the results you want and expect.

### Set the Dome9 compliance bundle to run using continuous compliance. 
If you're in single account mode, there needs to be a Continuous Compliance bundle per account. If not, select all the accounts that you set up cross-account roles in. 
Set the output topic as the ARN from the InputTopicARN one we set up
Set the format to be JSON - Full Entity


### ********* NOTE: ********** 
Currently Continuous Compliance sends a 'diff' for the SNS notifications. Because of this, if ran the bundle before, only new issues will be sent to SNS. 
If you want to have the first auto-remediation run to include all pre-existing issues, click "send all events" to force a re-send. 

For the compliance policy you have set up, look for a button on the right hand side with an arrow pointing up.  
![Send all events button](https://github.com/Dome9/cloud-bots/blob/master/docs/pictures/send_all_events_button.png?raw=true "Send all events button")

In this page, select SNS as the delivery method and your notification policy as the place to send the events.  
This can also be useful for rolling out new bots and/or testing since you can re-send the same event more than once.  
![Send all events page](https://github.com/Dome9/cloud-bots/blob/master/docs/pictures/send_all_events_page.png?raw=true "Send all events page")

### From here, you should be good to go!

## Questions / Comments
Contact: 



