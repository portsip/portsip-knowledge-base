# Auto Provisioning Security

## Introductions

**Auto-provisioning** simplifies the activation of phone services by allowing users to set up their phones through a web interface, eliminating the need for manual configuration. When a user activates their phone, the service provider automatically deploys the necessary settings for phone registration.

Here are the key points:

1. **Auto-provisioning**: This feature streamlines the process of setting up phones. Users can activate their phones via a web interface, avoiding the need to manually input configuration settings.
2. **PBX Configuration Files**: Typically, PBXs store extension phone configuration files in a designated folder. For instance, let’s consider the folder named `ac2fbb80c5da60e`. Within this folder, the PBX generates a configuration file for each phone, using the phone’s MAC address as the filename.
3. **Provisioning URL**: The PBX sends a provisioning URL to the IP Phone by a NOTIFY message that includes the configuration file URL. For example: `https://www.pbxhost.com/provision/ac2fbb80c5da60e`. The IP phone then appends its own MAC address to the URL, resulting in a specific configuration file URL, such as: `https://www.pbxhost.com/provision/ac2fbb80c5da60e/cc5ef641b794.xml` to download the configuration file.

## **Security Issue: Exposing User Credentials**

The typical implementation has a significant security flaw. If a user becomes aware of another IP phone’s MAC address, they can easily download that phone’s configuration files. These files contain sensitive information, including the user’s SIP extension number and password, stored in plain text(The IP Phone can only read the plain text from the configuration file and use the information to register to the PBX). As a result, user credentials are at risk of being leaked.

## **PortSIP Solution: Enhanced Security**

To avoid this security risk, PortSIP PBX takes a more secure approach. Instead of storing configuration files in a shared folder, PortSIP PBX creates a separate folder for each user. These folders have long, randomly generated names. Even if the MAC address is publicly known, guessing another user’s configuration file URL becomes practically impossible.

By implementing this approach, PortSIP significantly enhances security and protects user credentials. Robust security practices are crucial in any communication system, especially when dealing with sensitive data.

## **DHCP Option 66 and Auto-Provisioning in PortSIP PBX**

As previously mentioned, PortSIP PBX employs a unique approach to auto-provisioning in order to address security concerns. However, this approach introduces a minor issue for legacy IP phones that rely solely on [DHCP Option 66](provision-phone-using-dhcp-option-66.md) for provisioning.

Here are the key points:

1. **DHCP Option 66**: Some legacy IP phones only support provisioning via DHCP Option 66. This option requires entering a unified provisioning URL, which implies that all configuration files must be stored in the same folder.
2. **PortSIP’s Solution**: To accommodate  [DHCP Option 66](provision-phone-using-dhcp-option-66.md) while maintaining security, PortSIP offers a way to disable the creation of unique folders for each user. Here’s how to do it:
   * Sign in to the PortSIP PBX web portal.
   * Navigate to **Advanced > Settings** and click on the **General** Page.
   *   In the bottom section, paste the following string into the **Custom Options** field:

       ```
       {"disable_auto_provision_security" : true}
       ```
   * Click the **OK** button to save the changes.
3. **Effect of the Option**:
   * If you set the option to **false** or delete the above string, the PBX will continue to store configuration files in unique folders for each user.

Remember to adjust this setting based on your specific requirements. PortSIP’s flexibility allows you to balance security and compatibility with legacy devices.&#x20;

{% hint style="info" %}
We don't suggest you activate the disable\_auto\_provision\_security option that will bring the security issue.
{% endhint %}

