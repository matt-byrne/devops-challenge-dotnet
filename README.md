Read me provides a over view of what this project is about. 

What is this repository for?
This repository is used to build a containerized dotnet web application that connects to SQL database via Api call to provide business insights.

1) Directory structure as follows:
 Src - Holds the source codes for the project
 pipleline - Holds build deployment templates
 tests - Is the target for test case ran agains the code.
 scripts - Contains the migration script. 
 
2) Pipeline ( Azure pipeline) - Calls various YAML file from the template dir, it takes care of the complete restore/build process of the codes, code checks/scan are taken care by codeql, builds/publishes as Azure container image. 
 
3) Docker build
 This project has 2 containers one for the api server and for sql server. Hence docker-compose file was used over dockerfile to create containers.
 ACR is used to store the image, migration script is used for db related requirment before re-running the pipeline. 
