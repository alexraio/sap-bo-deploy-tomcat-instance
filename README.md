# sap-bo-deploy-tomcat-instance
Ansible playbooks to deploy extra tomcat instances taking the default SAP BO tomcat as template
## Intro
Ansible playbooks to configure one or more extra tomcat instances to run SAP Business Object applications. \
The Playbooks take the default SAP BO tomcat installation as template and generate a new tomcat instance with new settings.
The playbooks are tested on RHEL 7 and probably can work on any "systemd" distribution.
## Prerequisites
Systemd unit for the new tomcat instance must be already configured.
## Playbooks description
* deploy-sap-bo-tomcat.yml \
This Playbook copy the default SAP BO tomcat binary (only the binary) to a wished location
  ```
  Task List:
    play #1 (boserver01.pf.box): Deploy custom Tomcat instance based on existing binary installed in SAP BO	TAGS: []
    tasks:
      check if tomcat is running	TAGS: []
      End play if tomcat is running	TAGS: []
      continue if tomcat is NOT running	TAGS: []
      Archive default BO Tomcat binary	TAGS: []
      stat tomcat binary	TAGS: []
      Backup|move tomcat binary	TAGS: []
      make tomcat folder	TAGS: []
      Untar tomcat binary to /home/boadmin/thttpbot/tomcat.{{ tomcat_version }}	TAGS: []
      Create a symbolic link to tomcat version -> {{ tomcat_version }}	TAGS: []
      Change bobjenv.sh path in setenv.sh	TAGS: []
  ```
* deploy-tomcat-instance.yml
  This playbook create a custom Tomcat instance bei passing the instance as a variable.
  ```
  Task List:
    play #1 (boserver01.pf.box): Deploy custom Tomcat instance bossos1-2-3 or fassade	TAGS: []
    tasks:
      Check if Instance variable is defined	TAGS: []
      Continue if variable defined	TAGS: []
      set extra Tomcat facts if bosso1 instance	TAGS: []
      set extra Tomcat facts if bosso2 instance	TAGS: []
      Debug | list {{tomcat_instance}} instance extra facts	TAGS: []
      Check if tomcat is running	TAGS: []
      stop tomcat instance if running	TAGS: []
      systemd	TAGS: []
      delete tomcat instance if existing	TAGS: []
      generate tomcat instance directory if not exists	TAGS: []
      Archive default BO Tomcat Instace	TAGS: []
      Untar tomcat fefault instance to {{dest_tomcat}}/{{tomcat_instance}}	TAGS: []
      generate std missing directory in the tomcat instance {{tomcat_instance}}	TAGS: []
      Configure Server.xml of the instance	TAGS: []
      copy-deploy default BO war files	TAGS: []
      Debug | bo war file list	TAGS: []
      Start tomcat instance	TAGS: []
      Waiting till service is available	TAGS: []
      Stop tomcat instance	TAGS: []
      Delete war files	TAGS: []
   ```
## Usage
* ansible-playbook -i boinventory deploy-sap-bo-tomcat.yml
* ansible-playbook -i boinventory -e tomcat_instance=bosso2 deploy-tomcat-instance.yml 
