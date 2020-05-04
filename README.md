# OpenShift Guided Devops Cookbook

**Background**<br/>
This document/page defines the steps involved in  incorporating devops capability for the offer execution team.
This guide assumes you plan to develop your microservices in Java using Spring Boot framework  or Node.js/Angular.js
however, most of the steps remain same even if you are using a different technology e.g. Python
and you only need to follow the technology specific build, test and run toolset.

Please ensure you have been provided with Guided Development Kit which includes -

* OpenShift cluster URL & hostname
* Credentials for OpenShift and DMB

Process
The following flow-chart depicts the high level process including some of
the tools and IBM practices being demonstrated through the process.

![Devops](devops.png)

The platform setup, of creatign the various namespeces/project, installing the toolchains can also be handled as a part of this ansible script.For this the change required would be to ensure that the devops role gets executed by making change in apply.yml. Here we will uncomment the devops role and comment out the appsetup role,configchange role.

    - {role: 'devops',tags:'openshift'}
    - {role: 'appsetup', tags: 'appsetup'}
    - {role: 'configchange', tags: 'config'}
   
Then execute the command as 

<b>ansible-playbook -i inventory/hosts apply.yml</b>

For application specific pipeline creation and executation, the changes needs to be done in the inventory/hosts file. Specify the Git Url/Git token/Devops namespace name etc.

Now comment the devops role, configchange role and uncomment the appsetup role 

 - {role: 'devops',tags:'openshift'}
 - {role: 'appsetup', tags: 'appsetup'}
 - {role: 'configchange', tags: 'config'}
 
 Execute the command again as 

<b>ansible-playbook -i inventory/hosts apply.yml</b>

The pipeline will be created and will start the pipeline.

Finally if the application needs some change in its configurations, make these changes in the file on Git and provide these details in the hosts file as follows

isConfigChange=true
configFileName=<path to the configfile>
configAppChange=<Name of the application>
    
Now comment the devops role,appsetup role and uncomment the configchange role.

- {role: 'devops',tags:'openshift'}
- {role: 'appsetup', tags: 'appsetup'}
- {role: 'configchange', tags: 'config'}
 
 Execute the command again as 

<b>ansible-playbook -i inventory/hosts apply.yml</b>


**Conclusion**<br/>
This guide has given you an overview of how we would be using the devops on Openshift.
