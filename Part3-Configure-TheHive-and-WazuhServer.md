# Configuration of The Hive and Wazuh

This section covers the configuration steps for The Hive and Wazuh after their installation, including the setup of Cassandra and Elasticsearch for The Hive, and the integration of Wazuh for security data collection and incident response.

## Configuring The Hive

### Start with The Hive Configuration

After installing The Hive and its components like Cassandra and Elasticsearch, the first task is to configure The Hive to use these components effectively.

### Edit Cassandra's Configuration

Cassandra serves as the database for The Hive. You'll need to adjust Cassandra's configuration files for proper communication and functionality.

- **Open the Configuration File**: Use the command `nano /etc/cassandra/cassandra.yaml` to edit the file. (Note: The path might vary based on your installation.)
- **Cluster Name**: The `cluster_name` might be set to "Test Cluster" by default. You can change this name to something relevant to your setup.
- **Addresses Configuration**: Find the `listen_address` and `rpc_address`. Set `listen_address` to the server's IP where Cassandra is running, and `rpc_address` to allow connections.

### Customize Ports and Addresses

Ensure that the ports and addresses in the configuration file match your network setup and security policies, including specific ports for Cassandra and allowed connection addresses.

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
