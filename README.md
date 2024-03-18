# Ultimate Cybersecurity Lab

PFSENSE TO DO
*************


Enable SSH before connecting to firewall

Enable FreeBSD so we can pull down the wazuh agent

Enable the firewall logs (syslog already in place)

In Wazuh - Create group and enable rule





Enable Free BSD
****************
 
cd /usr/local/etc/pkg/repos/

Enter: vi pfsense.conf

FreeBSD: { enabled: yes }


cd /usr/local/etc/pkg/repos/
vi FreeBSD.conf

FreeBSD: { enabled: yes }




Install Agent
**************

pkg update
pkg search wazuh-agent

pkg install wazuh-agent-4.7.2




Start the firewall agent
************************
cp /etc/localtime /var/ossec/etc

edit ossec.conf - add

<server>
 <address>10.10.1.51</address>
</server>
 
 
 
sysrc wazuh_agent_enable="YES"

 
ln -s /usr/local/etc/rc.d/wazuh-agent /usr/local/etc/rc.d/wazuh-agent.sh


service wazuh-agent start







Enable Firewall logs
********************

Management > group create pfsense

edit group and add

<localfile>
	<log_format>syslog</log_format>
	<location>/var/log/filter.log</location>
</localfile>



Create custom rule
******************

Management > Rules > pfsense

Copy existing rule

pfSense-firewalllogs.xml

<group name="pfsense,">
  <rule id="87701" level="5" overwrite="yes">
    <if_sid>87700</if_sid>
    <action>block</action>
    <description>pfSense firewall drop event.</description>
    <group>firewall_block,pci_dss_1.4,gpg13_4.12,hipaa_164.312.a.1,nist_800_53_SC.7,tsc_CC6.7,tsc_CC6.8,</group>
  </rule>
</group>



Save







