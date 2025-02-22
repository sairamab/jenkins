##  Repo for Jenkins practice and projects ##


# Introduction #

Jenkins is an open source tool used widely for continous integration (CI) and continous deployment (CD), which helps is automating  builds, tests and deployment.

Why only Jenkins: 
  easy installation,
  architecture (Runs builds on multiple agents for scalability.) 
  wider plugins support(more than 1800!), 
  supports declarative and scripted pipelines, 
  various tools integration (many tools done through plugins).

How Jenkins works (Architecture):
  Jenkins Controller(Master) - handles UI, job scheduling, plugin management, triggering workflows, displaying results.
  Jenkins Agents(Slaves) - responsible for executing jobs as assigned by controller.
  Job - task which need to be executed


Creating these tasks can be done in multiple ways in jenkins, widely used one is with pipelines
  Declarative pipelines - structured, readable format and easier maintenance.
  Scripted pipelines - complex but more customizable.
  

# Installation #

Jenkins can be installed in multiple ways with java as prerequisite as it is java based application. Steps can be referred from documentation.
In this project, we installed jenkins through docker.

Jenkins Installation on windows through docker:
  docker to be installed in the form of docker desktop.
  docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
  post above step, we can access jenkins from local through http://localhost:8080
  we can connect to working environment through, docker exec -it -u root <container-id> /bin/bash
     <container-id> is obtained through checking running process of jenkins (docker ps)

 In the above setup, there is no separate agent. 
 As jenkins running in single container, by default jenkins runs it's jobs on same instance unless agent configured separately.
 If we want a master-slave architecture, we need to configure agent separately(In this case another container needs to be generated for agent).
 connection bwteen agent and master is done through SSH(linux agent) or JNLP jar(windows agent)


# Project #

A simple Jenkins file that performs CI process as : 
   cleaning up current working directory.
   pulls code from a public repository with maven project.
   builds with maven.
   performs test.

# Troubleshooting #

Before running the Jenkinsfile in this repository successfully, few builds were failed. Follow below to avoid those:
  1. Detected Maven Version: 3.8.7 is not in the allowed range [3.9.2,)
     Container is running with 3.8.7 maven version bur requirement is atleast 3.9.2. Tried updating maven version in container but it is saying upto date. So passed maven version from Jenkinsfile itself as

       tools {
           maven 'Maven-3.9.2' // Uses the Maven installed via Jenkins
       }

     set this maven version from jenkins UI:
       Dashboard -> Manage Jenkins -> Tools -> Maven Installations -> Add Maven
       give name as Maven-3.9.2(provide same in Jenkinsfile) and select Maven version as 3.9.2.

    This meets requirement condition on Maven version.