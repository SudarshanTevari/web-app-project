# Devops project: Deployment of Java web application.
* This project aims to teach you the implementation of CICD and GitOps for web applications using a range of DevOps tools including Git, Jenkins, SonarQube, Maven, Docker, Kubernetes and  Argo CD.
* This project is documented in gitbook, follow the link: https://sudrshan.gitbook.io/automation-of-web-app-deployment-on-kubernetes/

## Project Brief:
* Initially, developer will commit the code to GitHub repository, GitHub will trigger the code changes to Jenkins pipeline.
* Jenkins pipeline will be configured with the SonarQube which will Continuously Inspect the code for bugs, code smells, and security vulnerabilities through defined quality gates.
* Code will be packaged through code build using maven, later will copy the .war or .jar or .ear file i.e., maven build file into tomcat image, to achieve custom built image, this will be pushed to the docker hub repository.
* Kubernetes manifest file will be automatically updated with the latest docker image tag and will be pushed to configuration repository of GitHub.
* Argo CD a GitOps tool, will pull the changes and sync the application to desired state whenever the changes are made to the configuration repository.
* Once the CICD pipeline is finished, the complete process will be automated so that you can deploy the updated code with just one click by utilizing different deployment strategies of Kubernetes. Then the Argo CD will sync the application for desired state.
![image](https://user-images.githubusercontent.com/119477316/232227419-03a5896f-c5d3-4c0d-bf3c-5d54e1e937fc.png)

## Installation:
* To install Jenkins (follow official documentation): https://www.jenkins.io/doc/book/installing/
* To install docker (follow official documentation): https://docs.docker.com/engine/install/
* To deploy Sonarqube as docker container on Linux server, follow this link: https://gist.github.com/SudarshanTevari/9b34d654d3b8fad38884e4109ab1a8f3
* To install microk8s (follow official documentation): https://microk8s.io/docs/getting-started
* To install Argo CD on microk8s (follow official documentation): https://microk8s.io/docs/addons or follow my project Argo CD installation page
