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


# Create a MongoDB Database using MongoDB Atlas

1. Go to the link https://account.mongodb.com/account/register

2. Fill in the registration form with your information and click Create your Atlas account

<img src="Signup.png" alt="Sign Up" style="height: 100px; width:100px;"/>

3. Verify your Email ID

<img src="Verification.png" alt="Verification" style="height: 100px; width:100px;"/>

4. After successfully verification

<img src="Verify.png" alt="Verify" style="height: 100px; width:100px;"/>

5. You will be redirected here

<img src="Success.png" alt="Success" style="height: 100px; width:100px;"/>

6. On the next page select as per as image, and click the green Continue button.

<img src="Configuration.png" alt="Configuration" style="height: 100px; width:100px;"/>

7. On the next page select as per as image, and click the green Create button.

<img src="Next Configuration.png" alt="Next Configuration" style="height: 100px; width:100px;"/>

8. Wait sometime it will create your Shared Cluster

<img src="Shared Cluster.png" alt="Shared Cluster" style="height: 100px; width:100px;"/>

9. On the next Create a Shared Cluster page select as per as image and region which is near to you and then scroll down and give the name of the Cluster as shown in the image and click the green Create Cluster button

<img src="Advanced Configuration Part1.png" alt="Advanced Configuration" style="height: 100px; width:100px" />
<img src="Advanced Configuration Part2.png" alt="Advanced Configuration" style="height: 100px; width:100px" />

10. On the next page Create User with Username and Password by clicking on Create User button as shown in the image 

<img src="User.png" alt="User" style="height: 100px; width:100px" />

<img src="User Overview.png" alt="User Overview" style="height: 100px; width:100px" />

11. Go to the Network Access page and click the Add IP Address button

<img src="Network.png" alt="Network" style="height: 100px; width:100px" />

12. On the next page select the ALLOW ACCESS FROM ANYWHERE and then click the green Confirm button and it will configure the newtwork with Status Active as shown in the image

<img src="Network Confirmation.png" alt="Network Confirmation" style="height: 100px; width:100px" />

<img src="Network Status.png" alt="Network Status" style="height: 100px; width:100px" />
 
13. Go to the Database page and click the Connect button

<img src="Database.png" alt="Database" style="height: 100px; width:100px" />

14. And here click the Connect your application

<img src="Database Connection.png" alt="Database Connection" style="height: 100px; width:100px" />

15. And finally, select the Node.js Driver and Version and copy the connection string and keep it for later use and click the close button

<img src="Node.js Connection.png" alt="Node.js Connection" style="height: 100px; width:100px" />

16. All your Collections (Tables) that we create will be here

<img src="Final.png" alt="Final" style="height: 100px; width:100px" />


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

After this, Terraform is all done and your VM is ready

2. In the Google Cloud Console, on the Navigation menu, click Compute Engine > VM instances. The VM instances page opens and you'll see the VM instance you just created in the VM instances list.

# Configue Backend

1. SSH into your VM that created earlier 

2. It will open a command line shell of your VM in the new tab

3. Install Git 

```
sudo apt-get update -y
 
sudo apt-get install git -y
```
4. Install Node.js

```
sudo apt-get install -y curl
sudo apt-get install -y make
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```
5. Open Port 

```
gcloud auth login
gcloud config set project qwiklabs-gcp-04-808c237f1b07
gcloud compute firewall-rules create fw-fe  --allow tcp:2000
```

5. Git clone all your backend code using command

```
git clone https:// ...
```
6. Navigate to the main directory 

```
cd backend
```

- Open server.js and paste Connection String that you copied earlier for MONGO DB Connection and replace password with your password

```
nano server.js
```

7. Install PM2 which is a advanced process manager for production Node.js applications

```
sudo npm install -g pm2
```

8. Configure PM2 to run your backend Node.js App

```
pm2 ecosystem
vim ecosystem.config.js
```
- In the editor paste it

```
module.exports = {
    apps: [
        {
            name: 'nodejs-app-backend',
            script: './server.js',
            env: {
                NODE_ENV: 'production',
                PORT: 2000,
                HOST: '127.0.0.1'
            }
        }
    ]
};

```
9. Finally run PM2 server

```
pm2 start ./ecosystem.config.js
sudo pm2 startup
pm2 save
```
    
10. Copy the external IP address of your VM and use it as the base END point for your API



Congratulations! You are now able to create a Virtual Machine, API's, and Database for your backend.

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-13 | 0.1 | Shivam Kumar | Created new instructions for creating a Virtual Machine, API's, and Database|


## <h3 align="center"> All rights reserved. <h3/>


