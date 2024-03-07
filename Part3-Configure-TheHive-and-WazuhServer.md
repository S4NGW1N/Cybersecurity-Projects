# Configuration of The Hive and Wazuh

This section covers the configuration steps for The Hive and Wazuh after their installation, including the setup of Cassandra and Elasticsearch for The Hive, and the integration of Wazuh for security data collection and incident response.

## Configuring The Hive

### Start with The Hive Configuration

After installing The Hive and its components like Cassandra and Elasticsearch, the first task is to configure these components effectively.

### Edit Cassandra's Configuration

Cassandra serves as the database for The Hive. You'll need to adjust Cassandra's configuration files for proper communication and functionality.

- **Open the Configuration File**: Use the command `nano /etc/cassandra/cassandra.yaml` to edit the file. (Note: The path might vary based on your installation.)
 
- **Cluster Name**: The `cluster_name` might be set to "Test Cluster" by default. You can change this name to something relevant to your setup.
   ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/e7c2390c-0fe3-4525-8279-fc515797bee7)

  - **Addresses Configuration**: Find the `listen_address` and `rpc_address`. Set `listen_address` to the server's IP where Cassandra is running, and `rpc_address` to allow connections.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/6ab6e6c1-a586-4d4d-a268-8a53bf500784)

  - Next search for Seed_Provider and in the "seeds:" section change the local host IP to your Hive's Public IP address.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/664ce628-9cc1-408d-b2de-11047c82dd87)

  - Press Ctrl+ x to exit and hit Y to save your configuration then press enter.

- **Next step: Stop Cassandra Service**
```
systemctl stop cassandra.service
```
- **Remove Old Files**
```
rm -rf /var/lib/cassandra/*
```
- **Start up the service**
```
systemctl start cassandra.service
```

- **Optional: Check status to make sure service is running**
```
systemctl status cassandra.service
```
![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/3e765f9c-02a8-4da6-b9d5-425c9fa2ad17)


### Edit ElasticSearch's Configurations
Elastic Search will be used to manage data indices and query large amounts of data.

- **Open the Configuration file for Elastic Search**
  ```
  nano /etc/elasticsearch/elasticsearch.yml
  ```
  -  **Cluster Name**: The `cluster_name` might be set to "my-application" by default. You can change this to thehive.

  -  **Node Name**: Remove # and leave as node-1
  -   **Paths**: use default
    ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/d6d039e0-6801-4243-9a19-7c11bda127dc)

  - **Network**
    - "uncomment" by removing # from network.host and enter TheHive's public ipv4
    - Uncomment http.port:9200 and keep it on default.
      ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/d027dfa9-57f4-4bf0-82c5-9d562bc4fefd)

  - **Discovery Seed**
    - Uncomment Cluster.initial_master_nodes and delete "node-2" because we do not have a 2nd node.
      ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/f786fd9d-f865-423e-89a4-093f674a5662)

- **Start Service**
  ```
  systemctl start elasticsearch
  ```
  - **Enable Elastic Search**
    ```
    systemctl enable elasticsearch
    ```
      - **Check Service Status** 
        ```
        systemctl status elasticsearch.service
        ```
        Press "q" to exit.
        ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/a96a5a42-6e26-4dc5-ac82-c7dc81dc3ec3)

**Check Cassandra Service**
 - Double-check to make sure that cassandra's service is still running because sometimes it likes to stop when configuring elastic search.
```
systemctl status cassandra.service
```

### Edit TheHive's Configuration
  - **Give The Hive user and group access to the file path**. 
     - Check the file path by entering:
    ```
    ls -la /opt/thp
    ```
    - Below you can see that root has access to the hive directory
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/20495dc7-ac4d-4e51-b017-9179a62fc06b)
    - To change that enter:
      ```
      chown -R thehive:thehive /opt/thp
      ```
    - It's saying "Change owner to TheHive-user and TheHive-group over to the destination directories."
      - Check the file path again by entering:
    ```
    ls -la /opt/thp
    ```
    - We can now see that TheHive-user and TheHive-group have access to the directories.
    ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/f9d81767-ea01-4c68-bcae-38ce096b370e)


  - **The Hive's Configuration File**
    -Enter the command:
    ```
    nano /etc/thehive/application.conf
    ```
      - **Database and Index Configuration**
        - Update the IP address in the hostname section to your hive server's public IP address.
        - Update the cluster-name to the one you used for Cassandra. For me it will be "Test Cluster"
        - Update the IP address in the hostname section again to your hive server's public IP address.
          ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/2dc0f46d-d439-47c8-b85e-e3d4295998e7)
        - Under Service Configuration in application.baseURl, replace "localhost" with your Hive server's IP address.
          ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/bfd9c674-79b0-44af-9758-66ee40da3d98)
        - Ctrl+X then Y to save. Hit Enter to exit.

        - **Startup The Hive**
          - Enter the command:
            ```
            systemctl startup thehive
            ```
            ```
            systemctl enable thehive
            ```
          - Check the status of the hive to ensure it is up and running.
            ```
            systemctl status thehive
            ```
            ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/f176a518-aeb5-405b-99f3-d0f16487e89e)
            **Note**: You may have to sudo apt update to get the first two commands to work.
  
    - **TheHive Dashboard**
      - In your web browser enter http://thehiveserverIPaddress:9000
      -  Use the default credentials to log in
        - Username: admin@thehive.local
        - Password: secret
          ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/22d1413f-65eb-485d-b075-55d7ccf0a737)
          





## Configuring Wazuh

### Wazuh Server Setup

With The Hive configured, proceed to set up the Wazuh server. This involves configuring Wazuh to collect and analyze security data and integrate it with The Hive for incident response.

### Integration with The Hive

Integrate Wazuh with The Hive to automate alert forwarding and incident creation. This requires configuring Wazuh with The Hive's correct API endpoints and securing the communication between them.

### Windows 10 Client Reporting

To have a Windows 10 client report into Wazuh, install the Wazuh agent on the Windows machine, configure it with the Wazuh server's address, and ensure proper authentication.

## Testing the Configuration

After setup, test the configuration by generating alerts on the Windows 10 client and verifying that they appear in Wazuh and are forwarded to The Hive.

## Final Steps

- **Verify Operations**: Ensure both The Hive and Wazuh servers are operational and that alerts are correctly forwarded and processed.
- **Security Considerations**: Review your firewall and network security settings to secure your SOC environment.

This tutorial provides a foundational approach to configuring The Hive and Wazuh in your SOC Automation Project. For detailed steps, especially on integrating Wazuh with The Hive and configuring specific settings in Cassandra, refer to the official documentation of each tool.
