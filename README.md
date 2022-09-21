
# Configure IBM API Connect native OAuth provider with OpenLDAP as authentication registry

**Note**: The subdomain of the OpenShift cluster used in this example is:
```
.apps.ocp410.tec.uk.ibm.com
```
Please correct it to fit your environment.

## Setup OpenLDAP on OpenShift cluster

Clone the following repository with the sample configuration:
```
git clone https://github.com/jdiggity22/openldap.git
```

It includes a sample ldif file:
```
openldap/bootstrap/ldif/openldap-default.ldif
```
with the predefined users and groups:
```
├─ developers
│  ├─ todd
│  ├─ james
│  ├─ sarah
│  ├─ jane
│  └─ mike    
│   
├─ operations
│  ├─ bob
│  ├─ laura
│  ├─ josie
│  ├─ tom
│  └─ paula
│  
├─ support
│  ├─ carlos
│  ├─ jackie
│  └─ tony
│
└─ security
    ├─ janet
    ├─ frank
    └─ ted
```

.... **TODO:** complete this


## Setup LDAP user registry in API Connect


In your API Manager navigate to **Resources > User registries** and click on **Create**

<img width="850" src="images/Snip20220921_1.png">

Select **LDAP user registry**

<img width="850" src="images/Snip20220921_2.png">

<img width="850" src="images/Snip20220921_8.png">

<img width="850" src="images/Snip20220921_9.png">
































