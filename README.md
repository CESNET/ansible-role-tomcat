tomcat
======

Ansible Galaxy role [cesnet.tomcat](https://galaxy.ansible.com/cesnet/tomcat) that installs Tomcat accessible through 
Apache AJP proxy.

Requirements
------------

The directories listed in tomcat_readonly_paths and tomcat_readwrite_paths should exist. This roles
creates them if they don't, but does not set proper permissions and owners on them.

Role Variables
--------------
- tomcat_readonly_paths - list of directories outside of webapp folder where applications can read
- tomcat_readwrite_paths - list of directories outside of webapp folder where applications can read and write
- tomcat_override_template - name of template for generating systemd service override file, default is "override.conf.j2"
- tomcat_service_user - user owning the tomcat process, default is  "tomcat"
- tomcat_service_group - group owning the tomcat process, default is "tomcat"
- tomcat_server_xml_search_list - list of files, the first found is used to replace server.xml 

server.xml file
---------------

The role searches for the file server.xml in the list defined by the variable tomcat_server_xml_search_list. 
The default value makes it to first search for files/etc/tomcat9/server.xml on the playbook level, 
if not found, it will use the default files/server.xml file included in this role.

Example Playbook
----------------
```yaml
- hosts: all
  roles:
    - role: cesnet.tomcat
      vars:
        tomcat_readonly_paths:
          - "/etc/perun"
          - "/etc/mitreid"
        tomcat_readwrite_paths:
          - "/var/log/perun"
          - "/var/log/oidc"
        tomcat_service_user: "tomcat"
        tomcat_service_group: "perun"
```
