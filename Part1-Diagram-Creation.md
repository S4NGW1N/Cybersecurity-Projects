# Part 1: Project Diagram Creation

## Overview
In Part 1 of the SOC Automation Project, we focus on creating a comprehensive diagram that outlines the structure and components of our Security Operations Center (SOC) Homelab. This diagram serves as a blueprint for the setup and integration of various cybersecurity tools and systems.

## Components of the Diagram
The diagram includes the following key components:

### Wazuh Instance
- **Description:** Wazuh is an open-source security monitoring solution that provides intrusion detection, security analytics, and compliance checks.
- **Role in SOC:** Acts as the core for security monitoring, alerting, and incident response.

### SOAR Integration
- **Description:** Security Orchestration, Automation, and Response (SOAR) tools help automate and streamline security operations.
- **Role in SOC:** Enhances the efficiency of incident response through automation and orchestration of security tasks.

### TheHive
- **Description:** TheHive is an open-source, scalable security incident response platform.
- **Role in SOC:** Manages and tracks security incidents, facilitating collaboration and information sharing among security analysts.

### Network Infrastructure
- **Description:** Includes all network devices, such as routers, switches, and firewalls.
- **Role in SOC:** Provides the foundational network setup for the SOC, ensuring secure and efficient data flow.

### Endpoints
- **Description:** Represents various client devices, servers, and workstations in the network.
- **Role in SOC:** Sources of telemetry and log data for security monitoring and analysis.

## Data Flow
The data flow in the SOC Automation Homelab is as follows:

1. **Windows 10 Client (Wazuh Agent) to Wazuh Manager:**
   - The Windows 10 Client, equipped with the Wazuh Agent, monitors and collects security events. These events are sent through the network to the Wazuh Manager.

2. **Wazuh Manager Processing Events:**
   - The Wazuh Manager processes and analyzes the events to detect potential security incidents or threats.

3. **Wazuh Manager to Shuffle (OSINT):**
   - The Wazuh Manager generates alerts upon detecting suspicious activity, which are then forwarded to Shuffle for further enrichment using OSINT tools.

4. **Shuffle Enriching and Forwarding Alerts:**
   - Shuffle enriches the alerts with additional information from OSINT sources and forwards them to TheHive for case management.

5. **Alerts to TheHive for Case Management:**
   - TheHive uses the enriched alerts to create and manage security incident cases.

6. **Email Notifications to SOC Analyst:**
   - Shuffle sends email notifications about the alerts and incidents to the SOC Analyst.

7. **Response Actions by SOC Analyst:**
   - The SOC Analyst assesses the situation and prepares a response action, which is communicated back to Shuffle.

8. **Shuffle Coordinating Response Actions:**
   - Shuffle coordinates with the Wazuh Manager to execute the necessary response actions.

## Diagram Visualization
![SOC Automation Homelab Diagram](/SOC%20Automation%20Project%20Diagram.drawio.png)

## Importance of the Diagram
The diagram is crucial for:
- Understanding the overall structure and workflow of the SOC.
- Planning the setup and integration of security tools and systems.
- Serving as a reference for configuring and maintaining the SOC environment.

## Conclusion
This diagram lays the foundation for the subsequent parts of the SOC Automation Project, where we will delve into the installation, configuration, and operation of these components.
