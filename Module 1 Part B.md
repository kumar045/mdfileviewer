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



Congratulations! You are now able to use API Gateway

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-10 | 0.1 | Shivam Kumar | Created new instructions for creating a API Gateway|


## <h3 align="center"> All rights reserved. <h3/>

