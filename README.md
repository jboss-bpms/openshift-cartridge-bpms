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

**Access into the application shell**

To access into the application shell:

	rhc ssh <APPLICATION-ID>

**Manage users and roles**

When you install the cartridge, several users and roles are created. Their passwords are automatically generated for security reasons and displayed during cartridge installation. The following table summarizes this initial setup:

<table>
<tr>
	<th>User</th>
	<th>Description</th>
	<th>Roles</th>
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

* The application developer can show/add/remove/modify users and roles by using the <code>bpms-users.properties</code> and <code>bpms-roles.properties</code> files found in the cartridge git repository at <code>.openshift/config/</code> directory     
* Once users or roles files changed, do a <code>git push</code> in order to deploy the new files in the application container configuration path.         
* To display the current list of users and their passwords, you can simply run this command in your cartridge repository:

        cat .openshift/config/bpms-users.properties
    
* The current assignation of roles can be displayed by running this command in your cartridge repository:

        cat .openshift/config/bpms-roles.properties
        
* To create new users, change their passwords or role assignation, you must edit the following files in your cartridge repository.

        vi .openshift/config/bpms-users.properties
        vi .openshift/config/bpms-roles.properties

**Manage JBoss EAP configuration**

* The main configuration file for JBoss EAP is <code>standalone.xml</code>
* This file is available in your cartridge repository at location <code>.openshift/config/standalone.xml</code>
* Usefull for changing container configurations such as root logger level and so on

**Manage Maven configuration**

* There are two maven process builds involved in this cartridge:     
1.- Maven build for the developer cartridge git repository maven project     
    Build performed when pushing data into the cartridge git repository     
2.- Maven build for the BPMS webapp projects    
    Build performed when user hints <code>Build&Deploy</code> button in BPMS application     

* Both maven processes uses same Maven settings file available in your cartridge repository at location  <code>.openshift/config/settings.xml</code>
* User can use this Maven settings file to take control of the configuration for Maven builds, such as adding or removing external repositories. 

**Manage welcome root JBoss EAP web application**

* The developer cartridge git repository maven project generates a WAR artifact that is used as <code>welcome-root</code> webapp for the application URL.
* You can modify this application and when running a <code>git push</code> in your cartridge git repository, it is built and re-deployed in JBoss EAP.
* The generated WAR artifact is installed in the cartridge local repository, so it can be used as a Maven dependency too.

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

**Debugging application code**

If you need to debug your application code, you must enable the <code>enable_jpda</code> marker in the cartridge git repository:

    cd <app_name>/.openshift/markers/
    touch enable_jpda
    git add enable_jpda
    git commit enable_jpda -m "message"
    git push

Once the push is completed successfully, next step is to perform port forwarding:

    rhc port-forward <app_name>

You should see something similar to:

    Service Local               OpenShift
    ------- -------------- ---- ----------------
    java    127.0.0.1:3528  =>  127.1.244.1:3528
    java    127.0.0.1:4447  =>  127.1.244.1:4447
    java    127.0.0.1:5445  =>  127.1.244.1:5445
    java    127.0.0.1:8080  =>  127.1.244.1:8080
    java    127.0.0.1:8787  =>  127.1.244.1:8787
    java    127.0.0.1:9520  =>  127.1.244.1:9520
    java    127.0.0.1:9521  =>  127.1.244.1:9521
    java    127.0.0.1:9990  =>  127.1.244.1:9990
    java    127.0.0.1:9999  =>  127.1.244.1:9999

The debug port is <code>8787</code>. Now you can do remote debugging to <code>127.0.0.1:8787</code>.

**Example**

Follow this <a href="doc/Samples.md">guide</a> to get started with the BPMS application.


