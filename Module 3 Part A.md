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

- We also need to replace the backend url with the backend url of backend app that we deployed on the VM using terraform, and the API Gateway link to get current time

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

Builds are carried out using the Cloud Build service on the infrastructure of the Google Cloud Platform. Cloud Build can import source code from Cloud Storage, Cloud Source Repositories, GitHub, or Bitbucket, execute a build to your specifications, that are used for CI CD pipeline and can be triggered by changes in source code repository 

- Enable the App Engine API

[App Engine API](https://console.cloud.google.com/apis/library/appengine.googleapis.com?_ga=2.92569309.1154273686.1673849413-1685325301.1673849413&_gac=1.250146420.1673849413.CjwKCAiA5Y6eBhAbEiwA_2ZWIZjNG6PKtuHqB9Y06g7eQIP6bJMOYyeMt2kH8K9MQdstblrwnlH26RoCNWIQAvD_BwE)

## Required IAM permissions

Grant the App Engine Admin role and Service Account User to the Cloud Build service account:

1. Open the Cloud Build Settings page:

[Cloud Build Settings](https://console.cloud.google.com/cloud-build/settings?_ga=2.63336943.1154273686.1673849413-1685325301.1673849413&_gac=1.127569535.1673849413.CjwKCAiA5Y6eBhAbEiwA_2ZWIZjNG6PKtuHqB9Y06g7eQIP6bJMOYyeMt2kH8K9MQdstblrwnlH26RoCNWIQAvD_BwE)

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

c. Select the gcp-capstone repository that contains your source code and build config file.

d. Specify the regex for the branch or tag name that will start your trigger.

e. Configuration: Choose the build config file ` cloudbuild.yaml` you created previously.

Click Create to save your build trigger.

Anytime you push new code to your repository, you will automatically start a build and deploy on App Engine.

3. After the application is deployed, open the app in your web browser with the following URL:

```

http://<PROJECT_ID>.appspot.com

```
Note:

If you forgot your PROJECT_ID, run gcloud config list project from Cloud Shell.

Congratulations! You are now able to deploy a front end on App Engine with continuous integration and continuous deployment

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-15 | 0.1 | Shivam Kumar | Created new instructions for deploying a front end on App Engine with continuous integration and continuous deployment|


## <h3 align="center"> All rights reserved. <h3/>

