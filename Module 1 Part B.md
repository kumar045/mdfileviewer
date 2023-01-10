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
1. In Cloud Console, clone the Cloud Function sample repository:

```
git clone https:// ...
```

2. Change to the directory that contains the Cloud Functions sample code:

```
cd functions/helloworld
```

3.To deploy the function with an HTTP trigger, run the following command in the directory containing your function:

```
gcloud functions deploy helloGET --runtime nodejs12 --trigger-http --allow-unauthenticated
```


Congratulations! You are now able to use API Gateway

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-10 | 0.1 | Shivam Kumar | Created new instructions for creating a API Gateway|


## <h3 align="center"> All rights reserved. <h3/>

