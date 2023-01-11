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



