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