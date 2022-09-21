
# Configure IBM API Connect native OAuth provider with OpenLDAP as authentication registry


>**NOTE:** This document is written in hurry and it is in a very draft state. I apologize for all typos and inconsistencies. Hopefully, it will be improved later.



## Setup OpenLDAP on OpenShift cluster

Clone the following repository with the sample configuration:
```
git clone https://github.com/jdiggity22/openldap.git
```

You will find a sample *ldif* file in the following location:
```
openldap/bootstrap/ldif/openldap-default.ldif
```
It contains a several predefined users and groups. Please correct it to fit your requirements.

The base name is `dc=ibm,dc=com`. Admin user is `cn=admin,dc=ibm,dc=com` with password `Passw0rd`
Please see the configuration and correct it accordingly.




## Setup LDAP user registry in API Connect


In your API Manager navigate to **Resources > User registries** and click on **Create**

<img width="850" src="images/Snip20220921_1.png">

Select **LDAP user registry**

<img width="850" src="images/Snip20220921_2.png">

<img width="850" src="images/Snip20220921_8.png">

<img width="850" src="images/Snip20220921_9.png">
































