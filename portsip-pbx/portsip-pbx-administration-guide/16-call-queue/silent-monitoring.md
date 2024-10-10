# Silent Monitoring

Contact center monitoring is a practice of listening to sales or support agent calls in real-time or after-the-matter to check how well the call was handled. The practice of call center monitoring is essential to improve how agents communicate with customers.&#x20;

Call Center Monitoring = listening to sales or support calls in real-time or recorded

Benefits of real-time monitoring

* Supervisors can jump in & help struggling agents
* Junior agents get hands-on learning
* Customers get help immediately, no follow-up is needed
* More focused on immediate solutions for the customer

To learn how to monitor agents' calls using the Supervisor feature, follow the steps below:

## Enable Monitoring

Sign in to the PortSIP PBX Web portal, select the menu **Contact Center > Monitor Service**, and turn on the **Enable Monitor** option to enable the monitoring feature.

The monitoring service number is 888 by default, we suggest don't change it.

## Create the Monitor Group

Select the menu **Contact Center > Monitor Group**, and click the **Add** button to create the group.

* **Information** - Giving a friendly name to the monitor group
* **Enable** - Enable / Disable the monitor group
* **Group Members** - The agent who will be monitored in the group, click the **Add** button to add the members.
* **Supervisors** - The Supervisors who have permission to monitor the group members

## Silent Monitoring

For example, we would like to let supervisors 101 and 102 silence monitor other agents' 600 and 601 calls.

1. Create a new monitor group named **Queue Management** and turn on the **Enable** option.
2. Add users 600 and 601 as this group members.
3. Add users 101 and 102 as Supervisors of this group.
4. Click the **OK** button to save.

Now if extension 600 or 601 is on the call, then 101 or 102 can silence monitor 600 or 601 by dialing the FAC code with the extension number for example `*63600` or `*63601;`

During silence monitoring, the 101 or 102 can press DTMF 2 to whisper to the agent, press 3 to barge in, and press 4 to barge-break the call.

The 101 or 102 can dial `*63601*1` or `*63103`" directly to silence monitoring; By dialing `*68601*2` to whisper 600, dial `*68601*3` directly to barge-in, and dial `*68601*4` directly to barge-break the 601's call.
