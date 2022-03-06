# DevOps Challenge (.NET)

## Introduction :wave:

This Sales Api has two main endpoints to push the sales data and get the report.

### Folder Structure:

This follows the standard microsevice folder structure. 

* src contains all the source code
* tests contains all the unit test projects corresponsing to the source code projects
* pipeline contains all the build and deployment templates and scripts.
* .gitignore is designed such a way to manage the public files properly.
* docker compose file to build and push the multi container image.

### Pipeline Setup:

Pipeline is setup as muti stage pipeline which compirses of all the best parctice tooling such as build process, unit test execution, code coverage check and publish test execution report. It is also integrated with GitHub Advance Security to run security checks for the codebase. And when all those are passed it builds and publishes the Azure Container Image. It is based on Azure DevOps Multistage yaml pipeline.

Build step comprises of the below steps:

* Check for cached Nuget Package: If the restorable nuget packages are cached , it skip the Nuget Restore step to gain some speed in the build process.

* Restore Nuget Packages: If the restorable packages are not found in Cache, it will restore the packages with the details from Nuget.Config

* Building the Solution: Usual dotnet build for all the projects

* Execution Unit Tests: As per the inductry standard 75 - 80 % code needs to be covered with unit test scenarios. During this step all the test case in the test projects are executed and at the end it checks whether it is passing the 75 - 80 % test coverage criteria. If not it will fail the step.

* Generate Test Report: Install and configure the test report generator to produce the report for the executed test scenarios.

* Publish Test Report: Test report is published as artifacts to retrieve and later compare historical and future test data.

* Publish symbols to the Azure DevOps Artifacts / any sysmbol server: Publish the pdb files to symbol server for future issue debugging.

* Publish Infra Scripts: Publish the infra scripts as artifact to use during deployment stage.

As part of the tighting the Security, GitHub Advanced Security steps are alse added in pipeline which runs parallel to the build steps, to reduce the overall execution time. It detects and prevent of adding any security risks in the code and eliminate the current threats.

### Docker and image setup:

As this apllication requires mutiple containers to run at the sametime, we are using the docker compose tasks in Azure DevOps. We are here using Azure Container Registry to store and deploy images. As the SQL also need to be containerized, the migration sripts need to run before initializing the actual app code.

For the developers, as swagger definations are enabled they can add/update any new route, which will be visibile automatically. AS CI is enabled for this, they can push their code before merging to default branch. 

And DB related changes can be done in the migration scripts and recreate the image in pipeline.