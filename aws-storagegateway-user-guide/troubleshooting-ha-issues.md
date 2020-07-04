# Troubleshooting High Availability Issues<a name="troubleshooting-ha-issues"></a>

You can find information following about actions to take if you experience availability issues\.

**Topics**
+ [Health Notifications](#health-notifications)
+ [Metrics](#ha-health-notification-metrics)

## Health Notifications<a name="health-notifications"></a>

When you run your gateway on VMware vSphere HA, all gateways produce the following health notifications to your configured Amazon CloudWatch log group\. These notifications go into a log stream called `AvailabilityMonitor`\.

**Topics**
+ [You Get a Reboot Notification](#troubleshoot-reboot-notification)
+ [You Get a HardReboot Notification](#troubleshoot-hardreboot-notification)
+ [You Get a HealthCheckFailure Notification](#troubleshoot-healthcheckfailure-notification)
+ [You Get a AvailabilityMonitorTest Notification](#troubleshoot-availabilitymonitortest-notification)

### You Get a Reboot Notification<a name="troubleshoot-reboot-notification"></a>

You can get a reboot notification when the gateway VM is restarted\. You can restart a gateway VM by using the VM Hypervisor Management console or the Storage Gateway console\. You can also restart by using the gateway software during the gateway's maintenance cycle\.

**Action to Take**

If the time of the reboot is within 10 minutes of the gateway's configured [maintenance start time](MaintenanceManagingUpdate-common.md), this is probably a normal occurrence and not a sign of any problem\. If the reboot occurred significantly outside the maintenance window, check whether the gateway was restarted manually\.

### You Get a HardReboot Notification<a name="troubleshoot-hardreboot-notification"></a>

You can get a `HardReboot` notification when the gateway VM is restarted unexpectedly\. Such a restart can be due to loss of power, a hardware failure, or another event\. For VMware gateways, a reset by vSphere High Availability Application Monitoring can trigger this event\.

**Action to Take**

When your gateway runs in such an environment, check for the presence of the `HealthCheckFailure` notification and consult the VMware events log for the VM\.

### You Get a HealthCheckFailure Notification<a name="troubleshoot-healthcheckfailure-notification"></a>

For a gateway on VMware vSphere HA, you can get a `HealthCheckFailure` notification when a health check fails and a VM restart is requested\. This event also occurs during a test to monitor availability, indicated by an `AvailabilityMonitorTest` notification\. In this case, the `HealthCheckFailure` notification is expected\.

**Note**  
This notification is for VMware gateways only\.

**Action to Take**

If this event repeatedly occurs without an `AvailabilityMonitorTest` notification, check your VM infrastructure for issues \(storage, memory, and so on\)\. If you need additional assistance, contact AWS Support\. 

### You Get a AvailabilityMonitorTest Notification<a name="troubleshoot-availabilitymonitortest-notification"></a>

For a gateway on VMware vSphere HA, you can get an `AvailabilityMonitorTest` notification when you [run a test](Performance.md#vmware-ha-test-failover) of the [Availability and Application Monitoring](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html) system in VMware\. 

**Action to Take**

No action to take\.

## Metrics<a name="ha-health-notification-metrics"></a>

The `AvailabilityNotifications` metric is available on all gateways\. This metric is a count of the number of availability\-related health notifications generated by the gateway\. Use the `Sum` statistic to observe whether the gateway is experiencing any availability\-related events\. Consult with your configured CloudWatch log group for details about the events\.