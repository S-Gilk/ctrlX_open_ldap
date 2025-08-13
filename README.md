# ctrlX_open_ldap
Simple preconfigured Docker OpenLDAP server for CtrlX OS demonstration.

# How-To
1. Install docker engine
2. Run init_ldap.sh
	- WARNING: This will overwrite existing database
3. At the moment, permissions cannot be modeled from LDAP, so you will need to configure the group permissions locally on the CtrlX OS device. 
	- In the future, we may be able to download permissions from a remote directory.
	- I may add a REST API permission configuration script that a user can execute.
4. Run restart.sh to restart an existing LDAP server with persisted changes.

# CtrlX OS Configuration
- Authentication service = LDAP
- Server port = 1389
- Server type = Open LDAP
- Base DN = "dc=bosch,dc=com"
- Read-only user = boschrexroth
- Read-only password = boschrexroth
- User filter = (objectClass=person)
- Group filter = (objectClass=groupOfNames)
- Username attribute = uid
- Groupmembers attribute = member

# RBAC Structure
There are 3 groups with a single user in each group. 
- grp_engineer
- grp_maintenance
- grp_operator

# User Credentials
- engineer:engineer
- maintenance:maintenance
- operator:operator

# OpenLDAP Commands:
 -- View the OpenLDAP database config file to verify your LDAP settings (most will fall under the "/bitnami/openldap/slapd.d/cn=config" directory): 
 
 	cat /bitnami/openldap/slapd.d/cn\=config/olcDatabase\=\{2\}mdb.ldif

-- Verify all entries below your base DN (See groups, users, OUs, etc.):

  	ldapsearch -x -H ldap://192.168.1.8:1389 -D "cn=boschrexroth,dc=bosch,dc=com" -W -b "dc=bosch,dc=com" -s sub "(objectclass=*)"
 
-- View users:

  	ldapsearch -x -H ldap://192.168.1.8:1389 -D "cn=boschrexroth,dc=bosch,dc=com" -W -b "dc=bosch,dc=com" -s sub "(objectclass=inetOrgPerson)"
 
# Resources
https://hub.docker.com/r/bitnami/openldap
