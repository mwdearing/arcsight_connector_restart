CONNECTOR_RESTART
=========
Author: Michael Dearing

A role to restart and cleanup ArcSight Smart Connectors when either deployed as a software instance or on an appliance with SSH access. 

The role will prompt you for user-input so that the connector variable can be dynamic. You can also remove the below line and manually define the variable within your playbook. You can even define the variable with your playbook for looping. (See example.)
> tasks/main.yml
>> ```yaml
>> - import_tasks: set_connector.yml
>> ```

Features
----------
- Verify service is fully stopped before executing.
- Verify service is fully running before moving to the next connector. 
- Remove cache files older than 1 day.
- Remove old agent.properties.bak and agent.wrapper.conf.bak created by ArcMC
- Remove Threaddump fles.
- Remove Heapdump files.

TODO
-----
- Debug task: Removing java_*.hprof files.
- Integrate agentID cleanup script
- Ability to manage Windows hosted connectors. 

Requirements
--------------
None

Role Variables
--------------
| Name           	| Description                                               	| Default                                       	|
|:----------------  |-----------------------------------------------------------	|-----------------------------------------------:	|
| connector      	| Connector Folder Name. This var is populated with input    	| connector_1                                   	|
| connector_base 	| Full URI of connector install up to the current directory 	| /opt/arcsight/connectors/connector_1/current/ 	|
| days_to_delete 	| Delete cache files older than.                            	| 1d                                            	|
| service        	| Service naming structure for software deployments.        	| arc_connector_1                               	|
| appservice     	| Appliance service naming structure.                       	| arc_appliance_connector_1                     	|


Dependencies
------------
None

Example Playbook
----------------

> Using the ***set_connector.yml***
>>```yaml
>>
>>       - hosts: all
>>          become: yes
>>          roles: 
>>            - role: 'roles/connector_restart'
>>```

> Setting the variable in your playbook
>>```yaml
>>
>>       - hosts: all
>>          become: yes
>>          vars: 
>>              connector: "connector_1"
>>          roles: 
>>            - role: 'roles/connector_restart'
>>```

> Looping through multiple connectors.
>>```yaml
>>
>>       - hosts: all
>>          become: yes
>>          tasks:
>>            - name: Looping through connectors
>>              include_role:
>>                name: roles/connector_restart
>>              vars:
>>                connector: "{{ item }}"
>>              with_items:
>>                - "connector_1"
>>                - "connector_2"
>>                - "connector_3"
>>```

License
-------

MIT