## Steps to execute

- Create a branch from develop branch of repository `https://bitbucket.org/crestdatasys/xsoar-automation/src/develop/`.
  Format for branch name: `<integration_name>-automation`. 
- Modify configuration file as per integration requirements.
- In bamboo, navigate to `Plan Configuration` -> `Variables`.
- Update the following variables:
  
    - EMAIL_ID: email_id of the user.
    - REPORT_GENERATED_TO = `EMAIL`
    - integration_name = <integration_name> Ex. CofenseTriagev3
    - INTEGRATION_SRC = cofense-triage-xsoar-integration
    - INTEGRATION_SRC_BRANCH = `develop`
    - AUTOMATION_BRANCH_NAME = Created branch on the first step.

## Flow Diagram 

![flow diagram](flow_diagram.png)

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
