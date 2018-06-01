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

**Remark** : 

 - During the execution of this maven goal, the DeploymentConfig, Service and Route resources (yaml, json)
are created under the following directory `target/classes/META-INF/fabric8/openshift` for the 
individual files.
An OpenShift list is generated at the root of this folder `target/classes/META-INF/fabric8/`.
The tool will also generate k8s resource files 

 - No `BuildConfig` exists as it is created on-the-fly during `fabric8:build` goal execution

- Apply the resources

```bash
mvn fabric8:resource-apply
```

**Remark** : The DeploymentConfig, Service and Route resources are created on OpenShift within the namespace

- Build the OpenShift application

```bash
mvn fabric8:build
```
**Steps**

- During this maven goal, the fabric8 tool will generate first a BuildConfig and ImageStream resources
- Next, a S2I build will be then started using a SourceStragegy and where the content is pushed a binary stream to the platform
- So, the `target` directory is uploaded
- during the execution of this maven goal, a Docker tar is created containing a generated Dockerfile and the uber jar file.
- They both will be used to generate the application imagestream

- UnDeploy the application

```bash
oc delete svc,is,bc,dc,route -l app=ocp-fmp-build-install 
```