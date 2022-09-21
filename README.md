
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
Please check the configuration and correct it accordingly.

Navigate to the base folder and build the image. I am using `podman` in this example, but you can equally use `docker` as well. 

```
cd openldap

export NAME=osixia/extend-osixia-openldap
export VERSION=0.1.0

podman login docker.io

podman build -t $NAME:$VERSION --rm .
```  

Login to your OpenShift cluster and create project called "openldap":
```
oc new-project openldap
```  

>**NOTE:** In the followng steps, I am using the subdomain of my OpenShift cluster which is `.apps.ocp410.tec.uk.ibm.com` Please correct commands to use your subdomain.

Get the URL of OpenShift's internal registry:
```
oc registry info
```

In my case the result was:
```
default-route-openshift-image-registry.apps.ocp410.tec.uk.ibm.com
```
If your registry is not exposed to the external world you will have to create a route to expose it. Please check the OpenShift documentation.

Login to OpenShift image registry
```
podman login -u $(oc whoami) -p $(oc whoami -t) default-route-openshift-image-registry.apps.ocp410.tec.uk.ibm.com --tls-verify=false
```

List local images. The image that you have built before must appear on the list.
```
podman images | grep openldap
```

Tag this image to be able to push it to the OpenShift registry:
```
podman tag osixia/extend-osixia-openldap:0.1.0 default-route-openshift-image-registry.apps.ocp410.tec.uk.ibm.com/openldap/openldap:1.0.0
```

Push it:
```
podman push default-route-openshift-image-registry.apps.ocp410.tec.uk.ibm.com/openldap/openldap:1.0.0 --tls-verify=false
```

Apply the security setting to your OpenShift project
```
oc adm policy add-scc-to-group anyuid system:serviceaccounts:openldap
```

Create deployment and servce in your **openldap** project
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
 name: openldap
 labels:
   app: openldap
spec:
  replicas: 1
  selector:
    matchLabels:
      app: openldap
  template:
   metadata:
     labels:
       app: openldap
   spec:
     containers:
       - image: image-registry.openshift-image-registry.svc:5000/openldap/openldap:1.0.0
         imagePullPolicy: Always
         name: openldap
         ports:
           - containerPort: 389
             protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
 labels:
   app: openldap
 name: ldap-service
spec:
 ports:
   - name: openldap
     port: 389
 selector:
   app: openldap

```





## Setup LDAP user registry in API Connect


In your API Manager navigate to **Resources > User registries** and click on **Create**

<img width="850" src="images/Snip20220921_1.png">

Select **LDAP user registry**

<img width="850" src="images/Snip20220921_2.png">

<img width="850" src="images/Snip20220921_8.png">

<img width="850" src="images/Snip20220921_9.png">
































