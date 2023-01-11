**Estimated time needed:** 45 minutes

Welcome to the hands-on lab for **Cloud Monitoring**. In this lab, you will check performance, uptime, and overall health of the Instance.

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

# Create a Monitoring Metrics Scope

Set up a Monitoring Metrics Scope that's tied to your Google Cloud Project. The following steps create a new account that has a free trial of Monitoring.

- In the Cloud Console, click Navigation menu Navigation menu icon > Monitoring.

When the Monitoring Overview page opens, your metrics scope project is ready.

Install the Monitoring and Logging agents

The Cloud Monitoring agent is a collected-based daemon that gathers system and application metrics from virtual machine instances and sends them to Monitoring. By default, the Monitoring agent collects disk, CPU, network, and process metrics. 
The Monitoring agent allows third-party applications to get the full list of agent metrics.


1. Run the Monitoring agent install script command in the SSH terminal of your VM instance to install the Cloud Monitoring agent:

```
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
```

```
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

2. When asked if you want to continue, enter Y.

3. Run the Logging agent install script command in the SSH terminal of your VM instance to install the Cloud Logging agent:

```
sudo systemctl status google-cloud-ops-agent"*"
```

```
sudo apt-get update
```

# Create an uptime check

Uptime checks verify that a resource is always accessible. For practice, create an uptime check to verify your VM is up.

1. In the Cloud Console, in the left menu, click Uptime checks, and then click +Create Uptime Check.

2. Set the following fields:


Title: Uptime Check, then click Next.

Protocol: HTTP

Resource Type: Instance

Applies to: Single, gcp-capstone

Path: leave at default

Check Frequency: 1 min

Click on Next to leave the other details to default and click Test to verify that your uptime check can connect to the resource.

3. When you see a green check mark everything can connect. Click Create.

4. The uptime check you configured takes a while for it to become active. Continue with the lab, you'll check for results later. While you wait, create an alerting policy for a different resource.

# Create an alerting policy


Use Cloud Monitoring to create one or more alerting policies.

1. In the left menu, click Alerting, and then click +Create Policy.

Note: If required, click Try It! to use the updated alert creation flow.

2. Click on Select a metric dropdown. Disable the Show only active resources & metrics.

3. Type Network traffic in filter by resource and metric name and click on VM instance > Interface. Select Network traffic (agent.googleapis.com/interface/traffic) and click Apply. Leave all other fields at the default value.


4. Click Next.

5. Set the Threshold position to Above threshold, Threshold value to 500 and Advanced Options > Retest window to 1 min. Click Next.

6. Click on the drop down arrow next to Notification Channels, then click on Manage Notification Channels.

A Notification channels page will open in a new tab.

7. Scroll down the page and click on ADD NEW for Email.

8. In the Create Email Channel dialog box, enter your personal email address in the Email Address field and a Display name.

9. Click on Save.

10. Go back to the previous Create alerting policy tab.


11. Click on Notification Channels again, then click on the Refresh icon to get the display name you mentioned in the previous step.

12. Click on Notification Channels again if necessary, select your Display name and click OK.

13. Add a message in documentation, which will be included in the emailed alert.

14. Mention the Alert name as Inbound Traffic Alert.

15. Click Next.

16. Review the alert and click Create Policy.

You've created an alert! While you wait for the system to trigger an alert, create a dashboard and chart, and then check out Cloud Logging.

# Create a dashboard and chart

You can display the metrics collected by Cloud Monitoring in your own charts and dashboards. In this section you create the charts for the lab metrics and a custom dashboard.

In the left menu select Dashboards, and then +Create Dashboard.

Name the dashboard Cloud Monitoring Qwik Start Dashboard.


## Add the first chart

1. Click the Line option in the Chart library.

2. Name the chart title CPU Load.

3. Click on Resource & Metric dropdown. Disable the Show only active resources & metrics.

4. Type CPU load (1m) in filter by resource and metric name and click on VM instance > Cpu. Select CPU load (1m) and click Apply. Leave all other fields at the default value. Refresh the tab to view the graph.

## Add the second chart

1. Click + Add Chart and select Line option in the Chart library.

2. Name this chart Received Packets.

3. Click on Resource & Metric dropdown. Disable the Show only active resources & metrics.

4. Type Received packets in filter by resource and metric name and click on VM instance > Instance. Select Received packets and click Apply. Refresh the tab to view the graph.

5. Leave the other fields at their default values. You see the chart data.

# View your logs

Cloud Monitoring and Cloud Logging are closely integrated. Check out the logs for your lab.

1. Select Navigation menu > Logging > Logs Explorer.

2. Select the logs you want to see, in this case, you select the logs for the gcp-capstone instance you created:

- Click on Resource.

- Select VM Instance > gcp-capstone in the Resource drop-down menu.

- Click Apply.

- Leave the other fields with their default values.

- Click the Stream logs.

You see the logs for your VM instance.

## Check out what happens when you start and stop the VM instance.

To best see how Cloud Monitoring and Cloud Logging reflect VM instance changes, make changes to your instance in one browser window and then see what happens in the Cloud Monitoring, and then Cloud Logging windows.

1. Open the Compute Engine window in a new browser window. Select Navigation menu > Compute Engine, right-click VM instances > Open link in new window.

2. Move the Logs Viewer browser window next to the Compute Engine window. This makes it easier to view how changes to the VM are reflected in the logs

3. In the Compute Engine window, select the gcp-capstone instance, click the three vertical dots at the top of the screen and then click Stop, and then confirm to stop the instance.

It takes a few minutes for the instance to stop.

4. Watch in the Logs View tab for when the VM is stopped.

5. In the VM instance details window, click the three vertical dots at the top of the screen and then click Start/resume, and then confirm. It will take a few minutes for the instance to re-start. Watch the log messages to monitor the start up.

# Check the uptime check results and triggered alerts

1. In the Cloud Logging window, select Navigation menu > Monitoring > Uptime checks. This view provides a list of all active uptime checks, and the status of each in different locations.

You will see Uptime Check listed. Since you have just restarted your instance, the regions are in a failed status. It may take up to 5 minutes for the regions to become active. Reload your browser window as necessary until the regions are active.

2. Click the name of the uptime check, Uptime Check.

Since you have just restarted your instance, it may take some minutes for the regions to become active. Reload your browser window as necessary.

## Check if alerts have been triggered

1. In the left menu, click Alerting.

2. You see incidents and events listed in the Alerting window.

3. Check your email account. You should see Cloud Monitoring Alerts.


Congratulations! You are now able to use Cloud Monitoring to monitor performance, uptime, and overall health of the Instance.

## Author(s)
Shivam Kumar


## Change Log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-01-11 | 0.1 | Shivam Kumar | Created new instructions for Cloud Monitoring|


## <h3 align="center"> All rights reserved. <h3/>












