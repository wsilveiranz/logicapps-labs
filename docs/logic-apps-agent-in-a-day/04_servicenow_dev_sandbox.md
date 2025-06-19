# Module 4: Prequistes - ServiceNow setup
A ServiceNow Environment will be requirement to enable our integration to create and interact with incident tickets.
**Important:** Use your personal email address  when creating your ServiceNow Environment


1. Register a new account with ServiceNow:

    https://signon.service-now.com/x_snc_sso_auth.do?pageId=sign-up

1. Fill out the registration form:

    ![ServiceNow ID Signup](./images/02_01_signup_servicenow_id.png "Signup")

1. You will receive an Email Verification Code:

    ![Email Verfication Code](./images/02_02_email_verificaiton_code.png "Verify your account")

1. Navigate to the developer portal: 

     https://developer.servicenow.com/dev.do

    (if prompted, sign in using the account created above)

1. Request a Developer Instance

    ![Request Developer Instance](./images/02_03_request_developer_instance.png)

1. Click the Start Building link:

    ![Start building](./images/02_04_start_building.png)

## Configure OAuth Authentication 
We will need to configure an OAuth API endpoint to enable our logic apps to authenticate to our ServiceNow instance

1. Navigate to the `OAuth -> Application Registry` section
    - Using the top navigation menu
      - Select `All`
      - Enter `OAuth` in the search box
      - Select `Application Registery`

      ![Menu OAuth Application Registry](./images/02_05_menu_application_registry.png "menu oauth application registry")

1. Select `New`

    ![New Application Registry](./images/02_06_application_registry_new_button.png "new application registry")

1. Select `Create an OAuth API endpoint for external clients`
  
    ![Create OAuth API Endpoint](./images/02_07_create_oauth_api_endpoint_external_clients.png "create oauth api endpoint")

1. Configure the OAuth API endpoint as follows:
    - **Name:** `logic-apps-client`
    - **Client Secret:** [leave blank]
      - this will be auto generated on save
    - **Redirect URL:** https://logic-apis-northcentralus.consent.azure-apim.net/redirect
     - Click `Submit`

    ![Configure OAuth API Endpoint](./images/02_08_oauth_client_config.png "configure oauth api endpoint")

1. Save the `Client ID`, `Client Secret`  and the `Instance Name` created in this module for late steps when configuring the Logic Apps connection to ServiceNow
