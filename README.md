


Project Scope:

-	To set up SCM repository in GitHub to manage the code for Maven App
-	Maven will be setup to compile the code.
-	Setup three EC2 instance with Jenkins’s instance, configure the master worker/slave node.
-	Setup Tomcat on the same local host with the master node & Configuring the Tomcat username in context.xml
-	Setup the pipeline with a Groovy Script on the workflow orchestration

Step1. Provisioning of the SCM

Step2 
a.	provisioning of EC2 instances and setting up Jenkins on the instances
  ![image](https://user-images.githubusercontent.com/73761240/147131750-57962564-5fa5-4983-8ff0-6494f49ef4c1.png)       

b.	I setup the Configuration for Maven  and jdk in the Global Tool configuration

![image](https://user-images.githubusercontent.com/73761240/147131783-5d4b5689-0188-4f74-a145-e48f171a3ad2.png)
![image](https://user-images.githubusercontent.com/73761240/147131842-ec7a1140-52cb-461f-b884-3a37f3d2bf41.png)

Step 3 : Set up Master Slaves Nodes
 
a.	In the Configure Global Security
  Set the Agent port 9007 and the Remote root directory /opt/jenkins and executor 1
b.	I ran the following command 

sudo mkdir /opt/jenkins

cd opt/jenkins

   I downloaded this jar file sudo wget http://localhost:8081/jnlpJars/agent.jar into that directory and. And ran this command from the Node jenkins terminal
sudo java -jar agent.jar -jnlpUrl http://00.000.000.000:8080/computer/Slave1/jenkins-agent.jnlp -secret keycode -workDir "/opt/jenkins"



Repeated the step above for the second Slave Node


The three EC2 Instances are all configured on Master-worker/slave Architecture
![image](https://user-images.githubusercontent.com/73761240/147131894-e05a72db-78bc-4489-bc21-6d2bc32097a8.png)

Step4: Provision a Tomcat webserver

a.	 Downloaded the tar.gz Tomcat version9 from Apache Tomcat page
     https://tomcat.apache.org/download-90.cgi
 
      I cd into the bin file and made the start and shutdown.sh file executable
       Raise my elevation to superuser -> sudo su
       command -> Chmod +x startup.sh && Chmod +shutdown.sh

b.	My Jenkins is already running port 8080, I will configure Tomcat to run 8090
I updated the tomcat conf file 

Vim into the server.xml file and changed the connector port to 8090

c.	I also update the vim webapps/host-manager/METAINF/context.xml
              Comment out <!--  <Valve    
            className="org.apache.catalina.valves.RemoteAddrValve"
                       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />  -->
    
d.	In the manager file in webapps I repeated the process above

e.	 Setup Username and Password for the Tomcat

      vim conf/tomcat-users.xml and modified the username and password 
         <role rolename="manager-gui"/>
           <role rolename="manager-script"/>
               <user username="admin" password="admin" roles="manager-gui,manager-
                        script"
f.	Startup the Tomcat Server from the EC2 public DNS on port 8090 -> http://00.216.221.123:8090/   IP are dummy numbers and not the real numbers
     
![image](https://user-images.githubusercontent.com/73761240/147131935-d96f000b-ac24-406b-b636-a671adeed055.png)

 I setup the Jenkinsfile pipeline for the multiple stage build across the different Node
    See Jenkinsfile in the Git repository: https://github.com/erosons/demo

 I ran the build a shown below, my Test is still failing but working on it.






  
![image](https://user-images.githubusercontent.com/73761240/147131688-d275c86f-5cb0-4d50-9d4c-268f6d09fa95.png)
