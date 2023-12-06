# Rebranding PortSIP PBX

The PortSIP PBX provides a simple, but powerful rebranding feature that allows you to customize the PortSIP PBX Web Portal according to your preferences. Below, is the list of options or elements that you may customize with this add-on:

* PBX Name
* Company Name
* Website Link
* Logo
* PBX User Agent
* Company News
* WebRTC, Windows, Mobile Apps

### Company name, product name, website, logo

After signed the PortSIP PBX Web Portal, click the Menu "**Profile**", on the "**General**" page, you can enter the below item for rebranding.

* Website

On the "**Rebranding**" page, you can enter the below items for rebranding.

* Company Name
* Product Name
* Logo

### User Agent

You can set the customer the PortSIP PBX User-Agent by following the steps.

On the "**General**" page, click the Menu "**Profile**" > "**Advanced**," and in the "**User Agent"** field, type the User-Agent string you desire.

After you completed the above steps, then click the left menu "**WebRTC**", the WebRTC client will be launched, it's displayed with your company information.

### Company News

In the PortSIP PBX Web Portal, on the top right corner, it's always displayed the company news, PortSIP also supports you custom it to display your company news.

1. Download the JSON file from here: [https://portsip.com/news/portsip\_news.json](https://portsip.com/news/portsip\_news.json) and save it as  `news.json`
2. Edit the `news.json` file, replace the text and link - please keep in mind, just replace the text and link, don't change the JSON format
3. Save changes to the `new.json`, then upload it to your website, for example, [https://yoursite.com/news/new.json](https://yoursite.com/news/alctel\_new.json)

#### Linux

* If the PBX is installed on Linux, perform the below commands:

```
su root
vi /var/lib/portsip/system.ini 
```

* You will see the below content.

> \[global]\
> private\_ipaddr\_v4=\
> public\_ipaddr\_v4=\
> private\_ipaddr\_v6=\
> public\_ipaddr\_v6=\
> event\_url=[https://www.portsip.com/news/portsip\_news.json](https://www.portsip.com/news/portsip\_news.json)

&#x20;  &#x20;

* Change the [https://www.portsip.com/news/portsip\_news.json](https://www.portsip.com/news/portsip\_news.json) to    [https://yoursite.com/news/new.json](https://yoursite.com/news/new.json)
* Save the `system.ini`and exit the vi.    &#x20;
* Perform the below commands:

```
docker exec -it portsip-pbx /bin/bash
supervisorctl
restart gateway
exit
```

* Now the event news is changed.\


#### Windows

* Use Windows Notepad to open the C:\ProgramData\system.ini&#x20;
*   You will see the below content

    > \[global]\
    > private\_ipaddr\_v4=\
    > public\_ipaddr\_v4=\
    > private\_ipaddr\_v6=\
    > public\_ipaddr\_v6=\
    > event\_url=[https://www.portsip.com/news/portsip\_news.json](https://www.portsip.com/news/portsip\_news.json)


*   Change the [https://www.portsip.com/news/portsip\_news.json](https://www.portsip.com/news/portsip\_news.json) to    [https://yoursite.com/news/new.json](https://yoursite.com/news/new.json)

    &#x20;
* Save the `system.ini`and close Windows Notepad
* Go to Windows Service manager, restart the PortSIP Call Manager.

Now the event news is changed.

