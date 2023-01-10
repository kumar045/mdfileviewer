**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **API Gateway**. In this lab, you will create a API Gateway .


# Common Steps:

# Activate Cloud Shell

1. Click the Activate Cloud Shell icon at the top of the Google Cloud console.
2. When you are connected, you are already authenticated, and the project is set to your PROJECT_ID. The output contains a line that declares the PROJECT_ID for this session:

` 
Your Cloud Platform project in this session is set to YOUR_PROJECT_ID
`

3. gcloud is the command-line tool for Google Cloud. It comes pre-installed on Cloud Shell and supports tab-completion.

`
You can list the active account name with this command:
gcloud auth list
`

4. Click Authorize.

5. Your output should now look like this

`
ACTIVE: *
ACCOUNT: student-01-xxxxxxxxxxxx@qwiklabs.net
To set up the active account, run:
    $ gcloud config set account ACCOUNT
`
# Enable the required APIs

1. In the Cloud Console, on the Navigation menu (Navigation menu icon) click APIs & Services > Library.

2. Start typing "api gateway" in the Search bar, then select the API Gateway tile.

3. Now click the Enable button on the next screen.

#  Deploying an API backend

API Gateway sits in front of a deployed backend service and handles all incoming requests. In this lab, API Gateway routes incoming calls to a Cloud Function backend named helloGET that contains the function shown below

```
exports.helloGET = (req, res) => {
    res.send('Hello World!');
};
```
1. In Cloud Console, clone the Cloud Function sample repository

```
git clone https:// ...
```

2. Change to the directory that contains the Cloud Functions sample code

```
cd functions/helloworld
```

3.To deploy the function with an HTTP trigger, run the following command in the directory containing your function

```
gcloud functions deploy helloGET --runtime nodejs12 --trigger-http --allow-unauthenticated
```

# Test the API backend

1. When the function finishes deploying, take note of the httpsTrigger's url property or find it using the following command:

```
gcloud functions describe helloGET
```

2. To obtain your PROJECT_ID you can run the following command in the Cloud Shell console:

```
export PROJECT_ID=$(gcloud config get-value project)
```
3. Visit the URL to invoke the Cloud Function. You should see the message Hello World! as the response:

```
curl -v https://us-central1-${PROJECT_ID}.cloudfunctions.net/helloGET
```

# Create the API definition

API Gateway uses an API definition to route calls to the backend service. You can use an OpenAPI spec that contains specialized annotations to define the desired API Gateway behavior. The OpenAPI spec for this quickstart contains routing instructions to the Cloud Function backend.

1. From Cloud Shell, navigate back to your home directory:

```
cd ~
```

2.Create a new file named openapi2-functions.yaml:

```
touch openapi2-functions.yaml
```

3. Copy and paste the contents of the OpenAPI spec shown below into the newly created file:

```
# openapi2-functions.yaml
swagger: '2.0'
info:
  title: API_ID description
  description: API on API Gateway with a Google Cloud Functions backend
  version: 1.0.0
schemes:
  - https
produces:
  - application/json
paths:
  /time:
    get:
      summary: Get Time
      operationId: hello
      x-google-backend:
        address: https://us-central1-PROJECT_ID.cloudfunctions.net/helloGET
      responses:
       '200':
          description: A successful response
          schema:
            type: string
```

4. Set the following environment variables:

```
export API_ID="hello-world-$(cat /dev/urandom | tr -dc 'a-z' | fold -w ${1:-8} | head -n 1)"
```

5. Run the following commands to replace the variables set in the last step in the OpenAPI spec file:

```
sed -i "s/API_ID/${API_ID}/g" openapi2-functions.yaml
sed -i "s/PROJECT_ID/$PROJECT_ID/g" openapi2-functions.yaml
```

# Creating a gateway

Now you are ready to create and deploy a gateway on API Gateway.

1. From the Navigation menu, click API Gateway.

2. Click Create Gateway.

3. In the API section:

- Ensure the Select an API input is set to Create new API.

- For Display Name enter Hello World API

- For API ID, run the following command to once again obtain the API ID and enter it into the API ID field:

```
export API_ID="hello-world-$(cat /dev/urandom | tr -dc 'a-z' | fold -w ${1:-8} | head -n 1)"
echo $API_ID
```

4. In the API Config section:

- Ensure the Select a Config input is set to Create new API config.

- Do the following to upload the openapi2-functions.yaml file previously created.

5. In Cloud Shell, run the following command:

```
cloudshell download $HOME/openapi2-functions.yaml
```

6. Click Download.

7. Select Browse and select the file from the browser's download location:

- Enter Hello World Config in the Display Name field.

- Ensure the Select a Service Account input is set to Compute Engine default service account.

8. In the Gateway details Section:

- Enter Hello Gateway in the Display Name field.

- Set the Location drop down to us-central1.

9. Click Create Gateway.

# Testing your API Deployment

Now you can send requests to your API using the URL generated upon deployment of your gateway.

1. In Cloud Shell, enter the following command to retrieve the GATEWAY_URL of the newly created API hosted by API Gateway:

```
export GATEWAY_URL=$(gcloud api-gateway gateways describe hello-gateway --location us-central1 --format json | jq -r .defaultHostname)
```

2. Run the following command to ensure that the GATEWAY_URL environment variable is set:

```
echo $GATEWAY_URL
```

If it is not, that means you will need to wait longer for the API Gateway to be deployed.

3. Run the following curl command and validate that the response returned is Current Time:

```
curl -s -w "\n" https://$GATEWAY_URL/time
```

# Securing access by using an API key

To secure access to your API backend, you can generate an API key associated with your project and grant that key access to call your API. To create an API Key you must do the following:

1. In the Cloud Console, navigate to APIs & Services > Credentials.

2. Select Create credentials, then select API Key from the dropdown menu. The API key created dialog box displays your newly created key.

3. Copy the API Key from the dialog, then click on close.

4. Store the API Key value in Cloud Shell by running the following command:

```
export API_KEY=REPLACE_WITH_COPIED_API_KEY
```

Now, enable the API Key support for your service.

5. In Cloud Shell, obtain the name of the Managed Service you just created using the following command:

```
MANAGED_SERVICE=$(gcloud api-gateway apis list --format json | jq -r .[0].managedService | cut -d'/' -f6)
echo $MANAGED_SERVICE
```

6. Then, using the Managed Service name of the API you just created, run this command to enable API key support for the service:

```
gcloud services enable $MANAGED_SERVICE
```

# Modify the OpenAPI Spec to leverage API Key Security

In this section, modify the API config of the deployed API to enforce an API key validation security policy on all traffic.

1. Add the security type and securityDefinitions sections to a new file called openapi2-functions2.yaml file as shown below:

```
touch openapi2-functions2.yaml
```

2. Copy and paste the contents of the OpenAPI spec shown below into the newly created file:

```
# openapi2-functions.yaml
swagger: '2.0'
info:
  title: API_ID description
  description:  API on API Gateway with a Google Cloud Functions backend
  version: 1.0.0
schemes:
  - https
produces:
  - application/json
paths:
  /time:
    get:
      summary: Get Time
      operationId: hello
      x-google-backend:
        address: https://us-central1-PROJECT_ID.cloudfunctions.net/helloGET
      security:
        - api_key: []
      responses:
       '200':
          description: A successful response
          schema:
            type: string
securityDefinitions:
  api_key:
    type: "apiKey"
    name: "key"
    in: "query"
```

3. Run the following commands to replace the variables set in the last step in the OpenAPI spec file:

```
sed -i "s/API_ID/${API_ID}/g" openapi2-functions2.yaml
sed -i "s/PROJECT_ID/$PROJECT_ID/g" openapi2-functions2.yaml
```

4. Download the updated API spec file, you will use it to update the Gateway config in the next step:

```
cloudshell download $HOME/openapi2-functions2.yaml
```

5. Click Download.

# Create and deploy a new API config to your existing gateway

1. Open the API Gateway page in Cloud Console. (Click Navigation Menu > API Gateway.)

2. Select your API from the list to view details.

3. Select the Gateways tab.

4. Select Hello Gateway from the list of available Gateways.

5. Click on Edit at the top of the Gateway page.

6. Under API Config change the drop down to Create new API config.

7. Click Browse in the Upload an API Spec input box and select the openapi2-functions2.yaml file.

8. Enter Hello Config for Display Name.

9. Select Qwiklabs User Service Account for Select a Service Account.

10. Click Update.

# Testing calls using your API key

1. To test using your API key run the following command:

```
export GATEWAY_URL=$(gcloud api-gateway gateways describe hello-gateway --location us-central1 --format json | jq -r .defaultHostname)
curl -sL $GATEWAY_URL/time
```

You should see a response similar to the following error as an API key was not supplied with the curl call: UNAUTHENTICATED:Method doesn't allow unregistered callers (callers without established identity). Please use API Key or other form of API consumer identity to call this API.

2. Run the following curl command with the key query parameter and use the API key previously created to call the API:

```
curl -sL -w "\n" $GATEWAY_URL/hello?key=$API_KEY
```

If you do not have the API_KEY environment variable set you can get your API key from the left menu by navigating APIs & Services > Credentials. The key will be available under the API Keys section.

The response returned from the API should now be Current Time.

Congratulations! You are now able to use API Gateway to secure your API 

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-10 | 0.1 | Shivam Kumar | Created new instructions for creating a API Gateway|


## <h3 align="center"> All rights reserved. <h3/>

