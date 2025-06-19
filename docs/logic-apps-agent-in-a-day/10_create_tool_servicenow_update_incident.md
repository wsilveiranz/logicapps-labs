# Module 10: Create Tool - ServiceNow Update Incident
In this module we will create a stateful workflow to update an existing ServiceNow Incident. Next, we will incorporate the workflow into our Agent Loop at as tool to enable the agent to record work notes for our ServiceNow Incident.

## Create the Stateful Workflow
1. Search for and navigate to the Logic Apps service

    ![Search and Navigate to the Logic Apps Service](./images/10_01_search_bar_logic_apps.png "search and navigate to logics apps")


1. Open the Logic App created earlier 

    ![Open Logic App](./images/10_02_logic_apps_list.png ("open logic app"))

1. Create a new workflow
    - Click the `Workflows -> Workflows` from  the menu on the left
    - Click `+ Add -> Add`

      ![Create New Workflow](./images/10_03_create_new_workflow.png "create new workflow")

1. Create a new stateful workflow with:
    
    - **Workflow name:** `tool-ServiceNow-UpdateIncident`
    - Select the radio button for the `Stateful` worflow type
    - Click `Create`

    ![Create Stateful Workflow](./images/10_04_create_new_stateful_workflow.png "create new stateful workflow")

1. Open the worflow visual editor by clicking on the `tool-ServiceNow-UpdateIncident` link

    ![Open Workflow](./images/10_05_open_workflow.png "Open Workflow" )

## Configure Workfow
1. Configure the workflow trigger to accept an HTTP Request
    - Click on `Add Trigger`
    - Select the `Request` action located in the **Built-in tools** group

        ![Add Trigger - Request Action](./images/10_06_add_trigger_request_action.png "add trigger request action")
        
    - Select the `When a HTTP request is received`

        ![Select Action When a HTTP Request is Recieved](./images/10_07_add_action_when_a_http_request_is_received.png "select when a HTTP 
        request is received")

1. Configure the `When a HTTP request is recieved` action:
    - **Request Body JSON Schema**
        ```JSON
       {
            "type": "object",
            "properties": {
                "TicketNumber": {
                    "type": "string"
                },
                "Notes": {
                    "type": "string"
                }
            }
       }
       ```

1. Look up the internal identifier for the **Incident** in Service Now

    - Add a new action. Click `+ Add an action`

        ![Add an action](./images/10_08_add_a_action.png "add a action")

    - Select the `ServeNow - List Records` action

        ![Select Action ServiceNow List Records](./images/10_09_action_servicenow_list_records.png "servicenow list records")

1. Configure the List Records Activity as follows
    - Rename activity to `List Records - Get Ticket Details`
    - **Record Type:** `Incident`
    - **Advanced Parameters** (click `Show all`)
    - **Query:** `number=@{triggerBody()?['TicketNumber']}`

        (**note:** notice that the connection for the ServiceNow connection was automatically selected for the activity)

        ![ServiceNow List Activity Configuration](./images/10_10_servicenow_list_records_configuration.png "servicenow list records configuration")

1. Add the **Update Record** action to update the work notes on the incident in ServiceNow
    - Click on the `+` -> `Add a Action`
    - Search for `SearchNow` Connector and select the `Update Record` Activity

        ![ServiceNow Update Activity](./images/10_11_search_action_sevicenow_update_activity.png "servicenow update activity")

1. Configure the **Update Record** action
    - Rename activity to `Update Incident Work Notes`
    - **Record Type:** `Incident`
    - **System ID:** *(using the expression (fx) editor)* `first(body('List_Records_-_Get_Ticket_Details')?['result'])['sys_id']`
    - **State:** *(Advanced Parameter)* `2`
    - **Work Notes:**: *(Advanced Parameter)* `@{triggerBody()?['Notes']}`

        ![ServiceNow Update Activity Config](./images/10_12_update_activity_config.png "servicenow update activity config")

1. Add the **Response** activity to return a status message to the calling process
    - Click on the `+` -> `Add a Action`
    - Search for and select the `Response` activity

    ![Search Activity Response](./images/10_11_search_activity_response.png "search activity response")

1. Configure the **Response** activity
    - **Body:** 
        ```
        {
            "status": "Ticket {@{triggerBody()?['TicketNumber']}} has been updated successfully"
        }
        ```
    ![Response Activity Config](./images/10_13_response_activity_config.png "response activity config")

1. Save your workflow

    ![Save Workflow](./images/10_14_save_workflow.png "save workflow")



<div align="right" valign="middle">

**Previous:** <a href="09_add_tool_servicenow_create_incident_to_agentloop.md" title= "Previous: Module 9">Module 9: Create Tool - ServiceNow Create Incident</a>      

**Next:** <a href="11_add_tool_servicenow_update_incident_to_agentloop.md" title= "Next: Module 11">Module 11: Add ServiceNow Update Incident Tool to Agent Loop</a>      

</div>
