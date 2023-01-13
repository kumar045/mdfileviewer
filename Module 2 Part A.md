**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **Backend Development With Terraform**. In this lab, you will create a Virtual Machine, API's, and Database.


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
# Verifying Terraform installation

Terraform comes pre-installed in Cloud Shell.

- Open a new Cloud Shell tab, and verify that Terraform is available:

```
terraform
```

# Build infrastructure

The set of files used to describe infrastructure in Terraform is simply known as a Terraform configuration

1. In Cloud Shell, create an empty configuration file named instance.tf with the following command:

```
touch instance.tf
```

2. Click Open Editor on the Cloud Shell toolbar.

To switch between Cloud Shell and the code editor, click Open Editor or Open Terminal as required, or click Open in a new window to leave the Editor open in a separate tab.

3. Click the instance.tf file and add the following content in it, replacing <PROJECT_ID> with your Google Cloud project ID:

```
resource "google_compute_instance" "terraform" {
  project      = "<PROJECT_ID>"
  name         = "terraform"
  machine_type = "n1-standard-1"
  zone         = "us-west1-c"
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }
  network_interface {
    network = "default"
    access_config {
    }
  }
}
```
4. In Cloud Shell, verify that your new file has been added and that there are no other *.tf files in your directory, because Terraform loads all of them:

```
ls
```

# Initialization

1. Download and install the provider binary:

```
terraform init
```

2. Create an execution plan:

```
terraform plan
```

# Apply changes

1. In the same directory as the instance.tf file you created, run this command:

```
terraform apply
```

After this, Terraform is all done!

2. In the Google Cloud Console, on the Navigation menu, click Compute Engine > VM instances. The VM instances page opens and you'll see the VM instance you just created in the VM instances list.

# Configue Backend

1. SSH into your VM that created earlier 

2. It will open a command line shell of your VM in the new tab

3. Git clone all your backend code using command

```
git clone https:// ...
```
4. Navigate to the main directory 

```
cd backend
```






Congratulations! You are now able to create a Virtual Machine, API's, and Database for your backend.

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-13 | 0.1 | Shivam Kumar | Created new instructions for creating a Virtual Machine, API's, and Database|


## <h3 align="center"> All rights reserved. <h3/>


