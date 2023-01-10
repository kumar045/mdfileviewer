**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **Cloud Source Repositories**. In this part, you will create a Google Cloud Source Repository with all code files .


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
# Create a new repository
1. Start a new session in Cloud Shell and run the following command to create a new Cloud Source Repository named gcp-capstone

`
gcloud source repos create gcp-capstone
`
# Clone the new repository into your Cloud Shell session

1. Clone the contents of your new Cloud Source Repository to a local repo in your Cloud Shell session

`
gcloud source repos clone gcp-capstone
`
# Push to the Cloud Source Repository

1. Go into the local repository you created

`
cd gcp-capstone
`

2. Run the following commands to create a project file in your local repository

- Create a file called app.js with the command ` touch app.js ` that contains the following code
 
`
Demo Code
`


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
git commit -m "All files using Cloud Source Repositories"
`

4. Once you've committed code to the local repository, add its contents to Cloud Source Repositories using the git push command

`
git push origin master
`

# View all files in the Google Cloud repository

1. In the Console go to Navigation menu > Source Repositories


Congratulations! You are now able to use Google Cloud Source Repository

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-10 | 0.1 | Shivam Kumar | Created new instructions for creating a Cloud Source Repositories|


## <h3 align="center"> All rights reserved. <h3/>

