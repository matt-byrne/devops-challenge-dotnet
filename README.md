Read me provides a over view of what this project is about. 

1) Directory structure as follows:
 Src - Holds the source codes for the project
 pipleline - Holds build deployment templates
 tests - Is the target for test case ran agains the code.
 
2) Pipeline - Takes care of the restore/build process of the codes, code checks, test cases processes and publishes as Azure container image.
 It uses dotnet build to build the project, unit testing is done, a report is published.
 
3) Docker build
 This project has 2 containers one for the api server and for sql server. Hence docker-compose file was used over dockerfile to create containers.
 ACR is used to store the image, migration script is used db related requirment before re-running the pipeline. 
