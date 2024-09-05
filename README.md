# Configuring-Wazuh-Agent-with-Active-Response
This repository provides a step-by-step guide to configure Wazuh Agent with Active Response. Wazuh is an open-source security monitoring tool that can be used for intrusion detection, log analysis, and more. Active Response in Wazuh enables automatic actions in response to certain security events, helping to mitigate threats effectively.

## In this guide, you will learn how to:
1. Configure the Wazuh Manager to include an Active Response block.
2. Update local rules to trigger actions based on specific events.
3. Modify the Wazuh Agent configuration on Kali Linux to monitor changes and apply necessary updates.
4. Verify the configuration to ensure that Active Response and monitoring are functioning as expected.

## Steps Included
1. **Access Root User in Ubuntu**: Gain root privileges on the Wazuh Manager system.
2. **Verify Configuration File**: Ensure that the Wazuh configuration file is set up correctly.
3. **Add Active Response Block**: Modify the Wazuh configuration to include an Active Response block that triggers on specific rules.
4. **Update Local Rules Configuration**: Add rules to the local_rules.xml file to match configuration changes and trigger Active Response.
5. **Apply Changes**: Restart the Wazuh Manager to apply the updated configurations.
6. **Configuration Update on Kali Linux Agent**: Modify the agent's configuration to monitor changes in the Wazuh configuration file.
7. **Testing Configuration**: Validate the configuration changes by monitoring security events and verifying the Active Response functionality.


## Steps

### 1. **Access Root User in Ubuntu:** 
- Switch to the root user using the following command:
-Command:
```bash
sudo su
```
![image](https://github.com/user-attachments/assets/d8929049-2edb-4014-80f6-bf3818bef29a)


## 2. **Verify Configuration File:**
- Open the Wazuh configuration file using Nano:
- Command:
```bash
  nano /var/ossec/etc/ossec.conf
```
![image](https://github.com/user-attachments/assets/e062ca08-d57e-413a-9261-70c01085116e)

## 3. **Ensure the restart-wazuh Command Block is Present:**
- Confirm that the restart-wazuh command block is present in the configuration file.
![image](https://github.com/user-attachments/assets/9a40f1e0-abfe-44dc-ba55-bb5f4ffb3be8)

## 4. **Add Active Response Block:**
- Add the following block to the configuration file:
```bash
<active-response>
  <command>restart-wazuh</command>
  <location>local</location>
  <rules_id>100009</rules_id>
</active-response>
```
![image](https://github.com/user-attachments/assets/6430333a-010b-4461-9e1a-6aada634ab1b)

5. Save Configuration File
Use Ctrl+O to write out changes, then press Enter. Exit Nano using Ctrl+X.

6. Update Local Rules Configuration
Edit the local rules configuration file:
```bash
nano /var/ossec/etc/rules/local_rules.xml
Add the following rules:
<group name="restart,">
  <rule id="100009" level="5">
    <if_sid>550</if_sid>
    <match>ossec.conf</match>
    <description>Changes made to the agent configuration file - $(file)</description>
  </rule>
</group>
```
![image](https://github.com/user-attachments/assets/e5e1ea43-4328-4a41-bbee-47a3dd4d7631)

7. Save and Exit File
Save and exit the file after adding the rules.

8. Apply Changes by Restarting Wazuh Manager
Restart the Wazuh Manager to apply the configuration changes:
```bash
systemctl restart wazuh-manager
```

9. Configuration Update on Kali Linux Agent
Edit the agent configuration file on Kali Linux:
```bash
nano /var/ossec/etc/ossec.conf
```
10. Update Syscheck Configuration
Add the following line to the <syscheck> section:
```xml
<directories realtime="yes">/var/ossec/etc/ossec.conf</directories>
```
![image](https://github.com/user-attachments/assets/4813603f-5947-4d99-8f06-a71c0792a8b0)

Verify that changes were saved successfully.

Restart the Wazuh Agent on Kali Linux:
```bash
sudo systemctl restart wazuh-agent
```

11. Testing Configuration
In ossec.conf, under the <syscheck> section, add:

```xml
Copy code
<directories realtime="yes">/root</directories>
```
![image](https://github.com/user-attachments/assets/6b65f07f-fe22-4314-80e0-404cdbc273c3)

12. Test Configuration Verification
Verify the configuration changes on the Wazuh interface:

Navigate to the Kali agent's security events.
Filter by rule.id value.
Check if rule.id 100009 is present.

![image](https://github.com/user-attachments/assets/ba7ff363-6c1b-4eff-a487-ab9ccade6f42)
![image](https://github.com/user-attachments/assets/a1367390-4015-49aa-a7c3-ab99a9f661cd)

Conclusion
Follow these steps to configure Wazuh with Active Response and ensure your setup is functioning correctly. For further details or troubleshooting, refer to the Wazuh documentation or community forums.















