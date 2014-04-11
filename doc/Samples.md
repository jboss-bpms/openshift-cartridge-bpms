## Red Hat JBoss BPM Suite Cartridge for OpenShift

Example
-----

First thing to do is login as 'loan' user. Then selecting Authoring -> Project Authoring, the user access to the example assets.

<img title="ExploreProject" alt="ExploreProject" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/1_exploreProject.png">

To view the project assets the user has to navigate throw the Project explorer and open the project structure.

<img title="OpenFolder" alt="OpenFolder" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/2_openFolder.png">

The user can view the example assets selecting the type of asset. For example open the process model:

<img title="OpenAssets" alt="OpenAssets" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/3_openAssets.png">

Next step is do build&deploy the example selecting the Project Editor at menu. At this point, the user had to select the option Build&Deploy.

<img title="ProjectEditor" alt="ProjectEditor" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/4_projectEditor.png">

When the Build&Deploy has been successfully done, the next step is start a new instance. To do that, access to Process Management -> Process Definitions.
The new deployed process definition appears at list. Select 'start'.

<img title="StartInstance" alt="StartInstance" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/5_startInstance.png">

The process start form is shown. The user has to fill the fields. (for testing purpose, the field amortization must be 10 or 15, used at rule evaluation) and select
'Start Process'. At this point the process instance has been created.

<img title="ProcessInstance" alt="ProcessInstance" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/6_Start_processInstance.png">

Now the user can view the created instance selecting Process Management -> Process Instances.

<img title="InstanceDetails" alt="InstanceDetails" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/7_instanceDetails.png">

To view the path in the process model followed by the instance, the user can select 'Details'.
Selecting the view 'Process Model', the app will show the path followed by the instance through the model

<img title="InstancePath" alt="InstancePath" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/doc/example_img/8_InstancePath.png">








