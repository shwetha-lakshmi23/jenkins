JENKINS
--------
- a devops tool 
- used to automate build & deployement

CI -> Continuos Integration
whenever a developer makes som echanges in the code, build (using maven or gradle), and also can be tested 
So in the above case we can use jenkins to automate to set up a pipeline and execute the above tasks 

CD--> Continuous Delivery 
Here the devops team comes to picture , 
Once the test passes, they go ahead & deploy it 

		CI										CD
__________________________________________________ 			_________________________________________________
| 	Build,     Test 			 |			|Build Docker, push, Kubernetes, Deploy         |				
|						 |			|						|
|						 | 		        |						|
					         
Docker is an open platform for developing, shipping, and running applications. 
We use Docker as a platform to install jenkins - 1. extended platform to support many of the operating systems 2. speeds up installation process
Dont have to set up VM and all that if we are using docker
So we need to have the Docker installed
Download the Docker for mac & double click on it -- starts running 

docker run command for running the jenkins container
make sure the docker is running & then execuet the below command
docker run -p 8080:8080 -p 500000:50000 -v jenkins_home:/var/jenkins_home  jenkins/jenkins:lts
This will autmatically create jenkins_home which contains all the info related to jenkins
Exceute this command in the terminal (pull down the image )
Note : a password here

Now if you go to the browser & type localhost:8080- you will be able to see the UI :) 
give the password got in th prev step here

Code to the course - https://github.com/addamstj/Jenkins-Course

Create a new job 
Freestyle - like a blank slate , you can create whatever you want 
Pipeline - can be used as in like build - test - deploy 

For a pipeline job, when creating a new item, you will selecting the pipeline type
Under 'build triggers' in config section of the job, we have an option as poll SCM(Source Code Managemengt) [popular one ] => 
- when a developer does any change in git hub / any SCM, it will pick up the change and builds, tests!!

useful info about pipeline syntax : 
https://www.jenkins.io/doc/book/pipeline/syntax/
https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/


Some of the docker command :
1. docker run -p 8080:8080 -p 500000:50000 -v jenkins_home:/var/jenkins_home  jenkins/jenkins:lts
2. To list the docker containers
docker container ls 
3. 
docker exec -it -u root <container_id> /bin/bash
will give us a terminal at root at /bin/bash

inside root 
apt-get update
apt-get maven 

multibranch pipeline 
u can make jenkins scan multiple branches as well
General --> branch sources - git (paste the repo url )- it will automatically scan the feature branches under that job 
click on 'scan multibranch pipeline' in case your newly created branch name job is not launched. then your new branch job will appear under the job list :)

Using 'replay' you can edit the jenkins file at the job itself (but in companies that option will be disabled)

Parameterised pipeline
- allows us to pass the input before the job runs
for this , once we configure, we will have 'Build with parameters' option at the top left for us to pass the parameters

Jenkins envt var documentation: https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#using-environment-variables

Advanced Jenkins:
Grrovy : Behind the scenes, jenkins is using groovy
groovy syntax documentation : https://groovy-lang.org/syntax.html

Build health :
sunshine = all builds are successful
some cloudy =  worst health 
storm , rainy cloud etc.. if you hver on them , they will depict the status

Credentials: 
We can also apply AWS key secrets here and make the build more secure (can be includes in jenkins pipeline code)

Multiple Jenkin files:
We can have multiple jenkins file , but in the job we need to specify the correct location , so that it will consider the correct one 

Debugging trick:
Adding sleep statement 

If statement:
If we need to write naything in groovy, it must go to the 
script{}
above block , then inside which we can have our if statement

Functions
def funcName (datatype param 1){
}

and you can call this function in steps under stage section

Variable scope:
Scope is confined to only the block at which a variable is declared
This might be the cause for few of the failure. So when you encounter such errors, check for the variable scope
But in case you want a var to be available globally, use them within the 
'environment{}' block 

Multiple lines in bash shell, write like below in steps under stage 
sh """


"""

Build a job from a job 
insteps under one stage
write lik e
build '<name of the job from which you want to create this job>'

Pass parameters between jobs
 https://www.jenkins.io/doc/pipeline/steps/pipeline-build-step/

Plugins:
manage jenkins --> manage plugins

Clean up
stop the docker thats running
docker volume ls [lists the jenkins home]
docker container rm $(docker container ls -aq)
docker volume rm jenkins-home
********************************************************************