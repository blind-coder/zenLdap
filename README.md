zenLdap Introduction
====================
This is a Zenphoto-plugin that allows an LDAP-authentication against Microsoft Active Directory and OpenLDAP.
The login-function checks first if there is a local-db user available (for the given credentials), if not, it will try to authenticate against the given LDAP-directory

IMPORTANT NOTICE
================

If you installed this plugin before Dezember 7th 2020 you SHOULD IMMEDIATELY UPDATE!
There exists an error in the code of `alt/lib-auth.php` which allows users to log in with an empty password!

Setup
=====
Install the php-ldap extension, if you have apt:

 "apt-get install php5-ldap"
 
The plugin php script ldap_logon.php must be placed in "<zenphoto-directory>/plugins/ldap_logon.php".
In addition the lib-auth.php must be replaced - to do this create a folder named "alt" in the plugins directory

 "mkdir &lt;zenphoto-directory&gt;/plugins/alt/"
 
and place the alternative lib-auth.php in the alt-folder.

How it works
============
Basically you have to create and configure zenphoto-groups (through the zenplugin user_groups).
The zen-groups are read from both the memberOf attribute of the user and the cn attribute of group entries if the server doesn't support memberOf overlay.
During the login, the plugin receives the groups of the user. If there is a match between LDAP-group and Zenphoto-group, the zenphoto-group rights will be merged into the LDAP-user.
If there is no match, a default-template can be defined on the plugin-option page, than those rights will be used for the user.

Troubleshooting
===============
* Use the local-DB admin user to display the "debug.log" on the admin-page. It is most likely that there are some troubles regarding the given LDAP data.
* When creating a new OpenLDAP-user, you will have to use the user first somewhere else before you can use it as the Zenphoto-LDAP login (e.g. login into phpLDAPadmin)
* Use the tool "ldapsearch" to check your configured LDAP-properties on the plugin-option-page.
* "ldapsearch -h HOSTNAME -b 'dc=DOMAIN' -D 'cn=reader,dc=DOMAIN' -W -x" this will promt for the reader-password and then prints out lots of LDAP-stuff. The content of the -D option: cn=reader,dc=DOMAIN is right one for the "LDAP Reader DN"-textbox on the plugin-option-page

Known Problems
==============
Due to some reasons some images won't be created in the cache when the plugin is activated

Additional
==========
This plugin is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported License.
