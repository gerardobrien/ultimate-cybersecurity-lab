# Ultimate Cybersecurity Lab



## Episode 3 - Wazuh & Nessus build

In this episode we build our SIEM and XRD tool, Wazuh. We then install the Wazuh agent on our Kali virtal machine, our Docker server and our pfsense firewall.   We then move onto our vulnerability scanner, Nessus.

Please follow the instructions below to install the Wazuh agent on the pfsense firewall.


*******


### pfSense Wazuh Agent Install Overview

1. Enable SSH before connecting to firewall

2. Enable FreeBSD so we can pull down the wazuh agent

3. Enable the firewall logs (syslog already in place)

4. In Wazuh - Create group and enable rule



*******


### Enable FreeBSD so we can pull down the wazuh agent

By default, FreeBSD package repos are disabled on pfSense firewalls.  Follow below to enable for our lab.

SSH to firewall, navigate to the following directory:
```
cd /usr/local/etc/pkg/repos/
```

Edit pfsense.conf file:
```
vi pfsense.conf
```

Set to:
```
FreeBSD: { enabled: yes }
```

Edit freebsd.conf file:
```
vi FreeBSD.conf
```

Set to:
```
FreeBSD: { enabled: yes }
```


*******


### Install Wazuh Agent

Update the pacage cache
```
pkg update
```

SearcH for the Wazuh agent
```
pkg search wazuh-agent
```

Install Wazuh firewall agent
```
pkg install wazuh-agent-4.7.2
```

*******


### Start Wazuh Agent

Enter:
```
cp /etc/localtime /var/ossec/etc
```

The edit ossec.conf file:
```
vi ossec.conf
```

Add the following to the file, this is the address of your Wazuh server:
```
<server>
  <address>10.10.1.51</address>
</server>
```

 
Set the agent to enabled by entering the following:
```
sysrc wazuh_agent_enable="YES"
```

Create sumbolic link
```
ln -s /usr/local/etc/rc.d/wazuh-agent /usr/local/etc/rc.d/wazuh-agent.sh
```

```
service wazuh-agent start
```




*******


### Enable Firewall logs
Waht to monitor your firewall logs? We need to create a pfsense group, and add the following.

In Wazuh, navigate to Management > new new group

edit group and add the following:

```
<localfile>
	<log_format>syslog</log_format>
	<location>/var/log/filter.log</location>
</localfile>
```

*******


### Create custom rule
Monitor the output in filter.log file.

Navigate to Management > Rules > create a new rule and add the following:

```
<group name="pfsense,">
  <rule id="87701" level="5" overwrite="yes">
    <if_sid>87700</if_sid>
    <action>block</action>
    <description>pfSense firewall drop event.</description>
    <group>firewall_block,pci_dss_1.4,gpg13_4.12,hipaa_164.312.a.1,nist_800_53_SC.7,tsc_CC6.7,tsc_CC6.8,</group>
  </rule>
</group>
```
Add rule to the existing pfsense group you created previously.







