# Migrate the HA data

We may need to migrate the current HA to another HA in a different Data Center at times; this article will show you how to do so.



1.  Use SSH client connect to current HA master node, perform the command to ensure you are in the master node

    ```
     pcs status 
    ```
2.  On the master node, perform the below commands to stop it and pack the data

    ```
    pcs resource disable pbx 
    cd /var/lib/pbx/  
    tar cvfp data.tar.gz portsip/  
    ```
3. Now will need to copy the `data.tar.gz` to the `/root` folder of new HA master node.
4.  Start HA in current HA. (_this step is unnecessary if no need to run the current HA)_

    ```
    pcs resource enable pbx && pcs resource cleanup 
    ```
5.  Now in new HA master node, perform the below commands

    ```
    pcs resource disable pbx   
    cd /var/lib/pbx/      
    rm -rf portsip/      
    tar zxf /root/data.tar.gz    
    docker start pbx  
    ```
6.  The above steps will take a bit long time, use the below command to ensure the PBX is started

    ```
    docker logs pbx  
    ```
7.  if you don't want to config store the data to AWS S3, skip steps 7, 8, and 9. Perform the below command if the PBX is successfully started

    ```
    docker stop pbx  
    ```
8. Configure the AWS S3
9. Perform the command to start the HA:

```
pcs resource enable pbx && pcs resource cleanup  
```

10\. Now run the PBX setup wizard again to configure the PBX









