# Instructions

- Git clone project

```bash
git clone https://github.com/snowdrop/ocp-fmp-build-install.git && cd ocp-fmp-build-install
```

- Log on to OpenShift and create project

```bash
oc login $(minishift ip):8443 -u admin -p admin
oc new-project ocp-fmp-build-install
```

- Build locally

```bash
mvn clean package
```

- Create the OpenShift resources locally

```bash
mvn fabric8:resource
```

**Remark** : deploymentConfig, Service and Route are created under `target/classes/META-INF/fabric8/openshift` for the 
individual files like an OpenShift list `target/classes/META-INF/fabric8/openshift.yml` 

**WARNING** : No `BuildConfig` exists as it is created on-the-fly during `fabric8:build` phase

- Apply the resources

```bash
mvn fabric8:resource-apply
```

**Remark** : deploymentConfig, Service and Route are deployed on OpenShift

- Build the OpenShift application

```bash
mvn fabric8:build
```
**Steps**

- BuildConfig, ImageStream resources will be first created
- A build will be then started which is a S2I build and the content of the `target` directory uploaded (TO BE UPDATED)

```bash
ls -la target/docker/ocp-fmp-build-install/1.0/build
total 8
drwxr-xr-x  4 dabou  staff  128 Jun  1 07:04 .
drwxr-xr-x  5 dabou  staff  160 Jun  1 07:04 ..
-rw-r--r--  1 dabou  staff  106 Jun  1 07:23 Dockerfile
drwxr-xr-x  3 dabou  staff   96 Jun  1 07:04 maven
```
- UnDeploy the application

```bash
oc delete svc,is,bc,dc,route -l app=ocp-fmp-build-install 
```