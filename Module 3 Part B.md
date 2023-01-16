**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **Securing Web Applications with Web Security Scanner**. In this lab, you will scan the vulnerability using a Web Security Scanner scan.


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
# Create and run the scan

1. Click Navigation menu > App Engine > Security Scans.

2. Click + New Scan.

The Starting URLs field is prepopulated with the URL of the app we deployed on the App Engine.

4. Click Save to create the scan.

5. Click Run to start scanning:

Note: It takes some time to scan a frontend app that we have deployed.

# Check the results of the scan

After the scan completes, check the result of your scan in Security Command Center:

1. Click Navigation menu > Security > Security Command Center

2. Then click on the Findings tab and view the findings by Category and select XSS near the bottom of the list.

3. Click on one of the XSS findings to view its details.

4. In the Additional Information section, click the Source Properties tab:

If we have any vulnerability in frontend app then we'll notice the vulnerability.

Congratulations! You are now able to scan the vulnerability using a Web Security Scanner scan

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-10 | 0.1 | Shivam Kumar | Created new instructions for scanning the vulnerability using a Web Security Scanner scan|


## <h3 align="center"> All rights reserved. <h3/>

