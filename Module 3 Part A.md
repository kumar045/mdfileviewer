**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **Front End Deployment with CICD**. In this lab, you will deploy a front end on App Engine with continuous integration and continuous deployment.

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

# Clone the new repository into your Cloud Shell session

1. Clone the contents of your gcp-capstone Cloud Source Repository to a local repo in your Cloud Shell session

`
gcloud source repos clone gcp-capstone
`
# Push to the Cloud Source Repository

1. Go into the local repository you created

`
cd gcp-capstone
`

2. Run the following commands to create a project file in your local repository

- Create a file called app.yaml with the command ` touch app.yaml ` that contains the following instructions
 
```
runtime: nodejs18 # or another supported version
default_expiration: "102d 5h"

handlers:
  - url: /static
    static_dir: build/static

  - url: /(.*\.(json|ico|js))$
    static_files: build/\1
    upload: build/.*\.(json|ico|js)$

  - url: .*
    static_files: build/index.html
    upload: build/index.html
```
- Create a file called cloudbuild.yaml with the command ` touch cloudbuild.yaml ` that contains the following instructions

```
steps:
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    entrypoint: 'bash'
    args: ['-c', 'gcloud config set app/cloud_build_timeout 1600 && gcloud app deploy']
timeout: '1600s'
```

3. Commit the files using the following Git commands

`
git config --global user.email "you@example.com"
`

`
git config --global user.name "Your Name"
`

`
git add .
`

`
git commit -m "All deployment files using Cloud Source Repositories"
`

4. Once you've committed code to the local repository, add its contents to Cloud Source Repositories using the git push command

`
git push origin master
`

# View all files in the Google Cloud repository

1. In the Console go to Navigation menu > Source Repositories

# Deploying to the App Engine using CLoud Build for CI CD

- Enable the App Engine API

## Required IAM permissions

Grant the App Engine Admin role and Service Account User to the Cloud Build service account:

1. Open the Cloud Build Settings page:

- Go to the App Engine under GCP Service and set the status of the App Engine Admin role and the Service Account User role to Enabled as shown in the image

<img src="Cloud Build Settings page.jpg"
     alt="Cloud Build Settings page"
      />

2. Create a build trigger

- Open the Triggers page in the Google Cloud console:

- Select your project from the project selector drop-down menu at the top of the page.

- Click Open.

- Click Create trigger.

- On the Create trigger page, enter the following settings:

a. Enter a name for your trigger.

b. Select the repository event to start your trigger.

c. Select the repository that contains your source code and build config file.

d. Specify the regex for the branch or tag name that will start your trigger.

e. Configuration: Choose the build config file you created previously.

Configuration: Choose the build config file you created previously.




Congratulations! You are now able to deploy a front end on App Engine with continuous integration and continuous deployment

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-15 | 0.1 | Shivam Kumar | Created new instructions for creating a Cloud Source Repository|


## <h3 align="center"> All rights reserved. <h3/>

