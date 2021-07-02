## Overview
Prepare an automation script, so that in future demisto projects, QA could utilize it and reduce their manual verification of test cases.

Automation script will verify:

- Context CASE with api response, for a particular demisto command.
- All the negative test cases related to all the commands.
- Primary context key for all the commands.

## Prerequisites
### Bamboo Variables:

| VARIABLE NAME | POSSIBLE VALUES | DESCRIPTION | DEFAULT VALUE |
| --- | --- | --- | --- | 
| EMAIL_ID | Email id of any person or group email id of current project. | To send generated report of mismatched keys to this email. | NONE |
| REPORT_GENERATED_TO | SERVER, EMAIL | From the value of this variable bamboo plan will take decision weather to send report in email or to place report on \\10.0.1.253 server. | EMAIL |
| integration_name | Example: CofenseTriagev3 | Actual Integration name which is there in demisto repo and for which report will be generated. | NONE |
| INTEGRATION_SRC | Example: cofense-triage-xsoar-integration | Suppose your bitbucket repo link: `https://bitbucket.org/crestdatasys/cofense-triage-xsoar-integration/src/master/` Then value of this variable must be: `cofense-triage-xsoar-integration` | NONE |
| INTEGRATION_SRC_BRANCH | Branch name | Branch of your project repo | develop |
| AUTOMATION_BRANCH_NAME | Branch name | Branch created from XSOAR-automation repo | develop |

## Directory Structure:
- There would be all directories with the same name as commands name.
- In that directory where all possible JSON response from API would be available
- Content of JSON file structure would be as below
  ![Directory Structure](./doc_files/directory_structure.png)
- **command_prefix**: command's prefix that we have set in yml file.
- **api_response**: Here list or dict will be supported and value must be from the response which we are getting from actual api.

## Execution Steps:
1. User needs to create a branch from develop branch of repo: `https://bitbucket.org/crestdatasys/xsoar-automation/src/develop/`
   Format for branch name: `<integration_name>-automation`.
2. After creating a branch from User needs to create a directory inside `Automation` directory having name same as integration name for which user wants to run automation.
3. So you are having structure in your branch as `/Automation/your-integration-name/`.
4. At `/Automation/your-integration-name/` location users need to create directories having commands name for which automation will be performed.
5. Inside that all directories the user needs to place json files containing content for that particular command. Content is mentioned under the Directory structure section.
6. User need to modify configuration file as per integration requirements.
7. After that to perform automation user have to run bamboo plan with customised option. Plan link: `https://10.0.1.52:8443/browse/XA-FMK`
8. In customised option user have to pass values of all needed bamboo variables according to their plan. Details of bamboo variables given at bamboo variables section inside Prerequisites.
9. And then if user has passed value of REPORT_GENERATED_TO bamboo variable = SERVER then user needs to check report at server = 10.0.1.253
10. Or else if user has passed value of REPORT_GENERATED_TO bamboo variable = EMAIL then email containing document as report will be sent to the email address which is set in bamboo variable EMAIL_ID.


## Flow Diagram

![flow diagram](./doc_files/flow_diagram.png)

- Upload automation script `TestingScript` to the demisto instance.
- Create an incident to the demisto instance.
- For negative test cases execute all the cases to the demisto instance and verify that all the error messages are as expected.
- Steps to validate primary key:
    
    - Execute command.
    - Modify the context data with dummy values except primary key field value.
    - Re-execute the command.
    - Verify the context data.
- Prepare a document for all the cases.

## Automation Script - TestingScript

Generic script to be used for testing outputs and primary keys of all the commands.

## Script Data

| **Name** | **Description** |
| --- | --- |
| Script Type | python3 |
| Tags | testing |

## Inputs

| **Argument Name** | **Description** |
| --- | --- |
| command | Command to be tested. |
| arguments | Arguments related to the command |
| change_context | Whether user wants to change the context data after execution of the command or not. |
| primary_key | Primary key of the context data. |
| output_prefix | Output prefix for the context path. |
| get_context | Can be made to True if user wants to only retrieve the context data. |

## Outputs

There are no outputs for this script.
