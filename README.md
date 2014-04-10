## Red Hat JBoss BPM Suite Cartridge for OpenShift

Summary
-------
This cartridge provides the **_Red Hat JBoss BPM Suite_** for easy deployment to OpenShift.

Deployment
----------

To try out JBoss BPM Suite on OpenShift please follow the instructions:

If you want to use the [OpenShift create application page](https://openshift.redhat.com/app/console/application_types), enter the cartridge URI of **https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/metadata/manifest.yml** in the entry field (at the bottom left of the form).

Or if you want to use the [rhc command line](https://www.openshift.com/developers/rhc-client-tools-install) type:

    rhc create-app -g medium <APP NAME> https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/metadata/manifest.yml

This will output the generated users and passwords for Business Central.

You can use them to login into Business Central or BAM applications.


Usage
-----

**Access the web application**

* In order to access the BPMS Business Central navigate to <code>http://&lt;app_name&gt;-&lt;rh_domain&gt;.&lt;domain&gt;:&lt;port&gt;/business-central</code>   
* In order to access the BPMS Dashbuilder navigate to <code>http://&lt;app_name&gt;-&lt;rh_domain&gt;.&lt;domain&gt;:&lt;port&gt;/dashbuilder</code>

**Clone Git repositories from the web application**

* All repositories present in the web application can be cloned    
* OpenShift applications have opened by default the HTTPS port (8443).    
* If your OpenShift client requires accessing other application public ports, you must perform port forwarding in your station.     
* So in order to clone a specific web application repository, first you must perform port-forwarding from the OpenShift server and then run the <code>clone</code> git command (when ports are forwarded).    

These are the steps:    
   
1.- Using your <code>rhc</code> client, type the following command: <code>rhc port-forward &lt;APP_NAME&gt;</code>       

2.- When executing the above command, a list of forwarded ports will be visible. This ports are forwarded until you finish the rhc client process (executed using <code>port-forward</code> argument). So DO NOT close this terminal until the application repository is fully cloned.     

3.- Once OpenShift application ports are forwarded, you can run <code>git clone</code> using this URL: <code>ssh://&lt;app_user&gt;@127.0.0.1:9521/&lt;app_name&gt;</code> (NOTE that the user for the ssh connection is not a system user, is a BPMS application user)     

This is an example of cloning a application repository named _test-repo_ using a BPMS application user with login _bpm-admin_. The application name is _test-app_:

    rhc port-forward test-app
    git clone ssh://bpm-admin@127.0.0.1:9521/test-repo

IMPORTANT NOTE: If when trying to clone the application repository you see this error:   

    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that the DSA host key has just been changed.
    The fingerprint for the DSA key sent by the remote host is
    e7:f6:52:7d:fd:1c:fd:31:5e:a9:8c:bc:6f:d3:19:ed.
    Please contact your system administrator.
    Add correct host key in ~/.ssh/known_hosts to get rid of this message.
    Offending key in ~/.ssh/known_hosts:5
    DSA host key for [X.Y.Z.Q]:9521 has changed and you have requested strict checking.
    Host key verification failed.
    fatal: The remote end hung up unexpectedly

You will have to remove the existing fingerprint for your ssh connection in your <code>~/.ssh/known_hosts</code> file.

**Manage users and roles**

When you install the cartridge, several users and roles are created. Their passwords are automatically generated for security reasons and displayed during cartridge installation. The following table summarizes this initial setup:

<table>
<tr>
	<td>User</td>
	<td>Description</td>
	<td>Roles</td>
</tr>
<tr>
	<td>bpm-admin</td>
	<td>BPM Administrator</td>
	<td>admin</td>
</tr>
<tr>
	<td>bpm-analyst</td>
	<td>Process analyst role</td>
	<td>analyst</td>
</tr>
<tr>
	<td>bpm-manager</td>
	<td>BPM Manager</td>
	<td>manager</td>
</tr>
<tr>
	<td>bpm-user</td>
	<td>BPM User</td>
	<td>user</td>
</tr>
<tr>
	<td>root</td>
	<td>Dashboard superuser</td>
	<td>admin</td>
</tr>
<tr>
	<td>loan</td>
	<td>User to run the mortgage example</td>
	<td>analyst,broker,manager,appraiser</td>
</tr>
</table>


To display the current list of users and their passwords, you can simply use the following RHC command:

	rhc ssh <APPLICATION-ID>  'cat bpms/standalone/configuration/bpms-users.properties'

The current assignation of roles can be displayed by using:

	rhc ssh <APPLICATION-ID>  'cat bpms/standalone/configuration/bpms-roles.properties'

To create new users, change their passwords or role assignation, you must edit the following files inside the gear.
	
	bpms/standalone/configuration/bpms-users.properties
	bpms/standalone/configuration/bpms-roles.properties

To access into the gear type:

	rhc ssh <APPLICATION-ID>

Example
-----

First thing to do is login as 'loan' user. Then selecting Authoring -> Project Authoring, the user access to the example assets.

<img title="ExploreProject" alt="ExploreProject" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/1_exploreProject.png">

To view the project assets the user has to navigate throw the Project explorer and open the project structure.

<img title="OpenFolder" alt="OpenFolder" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/2_openFolder.png">

The user can view the example assets selecting the type of asset. For example open the process model:

<img title="OpenAssets" alt="OpenAssets" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/3_openAssets.png">

Next step is do build&deploy the example selecting the Project Editor at menu. At this point, the user had to select the option Build&Deploy.

<img title="ProjectEditor" alt="ProjectEditor" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/4_projectEditor.png">

When the Build&Deploy has been successfully done, the next step is start a new instance. To do that, access to Process Management -> Process Definitions.
The new deployed process definition appears at list. Select 'start'.

<img title="StartInstance" alt="StartInstance" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/5_startInstance.png">

The process start form is shown. The user has to fill the fields. (for testing purpose, the field amortization must be 10 or 15, used at rule evaluation) and select
'Start Process'. At this point the process instance has been created.

<img title="ProcessInstance" alt="ProcessInstance" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/6_Start_processInstance.png">

Now the user can view the created instance selecting Process Management -> Process Instances.

<img title="InstanceDetails" alt="InstanceDetails" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/7_instanceDetails.png">

To view the path in the process model followed by the instance, the user can select 'Details'.
Selecting the wiew 'Process Model' the app show the path followed by the instante throw the model

<img title="InstancePath" alt="InstancePath" src="https://raw.githubusercontent.com/jboss-bpms/openshift-cartridge-bpms/master/example_img/8_InstancePath.png">








