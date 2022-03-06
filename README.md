# DevOps Challenge (.NET)

## Introduction :wave:

This Sales API has two main endpoints to push the sales data and get the report.

### Folder Structure:

This follows the standard microservice folder structure. 

* src contains all the source code
* tests contains all the unit test projects corresponding to the source code projects
* pipeline contains all the build and deployment templates and scripts.
* .gitignore is designed in such a way to manage the public files properly.
* docker-compose file to build and push the multi-container image.

### Pipeline Setup:

The pipeline is set up as a multi-stage pipeline which comprises all the best practice tooling such as build process, unit test execution, code coverage check and publishes test execution report. It is also integrated with GitHub Advanced Security to run security checks for the codebase. And when all those are passed it builds and publishes the Azure Container Image. It is based on the Azure DevOps Multistage YAML pipeline.

Build step comprises of the below steps:

* Check for cached Nuget Package: If the restorable NuGet packages are cached, it skips the Nuget Restore step to gain some speed in the build process.

* Restore Nuget Packages: If the restorable packages are not found in Cache, it will restore the packages with the details from Nuget.Config

* Building the Solution: Usual dotnet build for all the projects

* Execution Unit Tests: As per the industry standard 75 - 80 % of code needs to be covered with unit test scenarios. During this step, all the test cases in the test projects are executed and at the end, it checks whether it is passing the 75 - 80 % test coverage criteria. If not it will fail the step.

* Generate Test Report: Install and configure the test report generator to produce the report for the executed test scenarios.

* Publish Test Report: Test report is published as artifacts to retrieve and later compare historical and future test data.

* Publish symbols to the Azure DevOps Artifacts / any symbol server: Publish the PDB files to the symbol server for future issue debugging.

* Publish Infra Scripts: Publish the infra scripts as artifacts to use during the deployment stage.

As part of tightening the Security, GitHub Advanced Security steps are also added in the pipeline which runs parallel to the build steps, to reduce the overall execution time. It detects and prevents adding any security risks in the code and eliminate the current threats.

### Docker and image setup:

As this application requires multiple containers to run at the same time, we are using the docker-compose tasks in Azure DevOps. We are here using Azure Container Registry to store and deploy images. As the SQL also need to be containerized, the migration scripts need to run before initializing the actual app code.

For the developers, as swagger definitions are enabled they can add/update any new route, which will be visible automatically. AS CI is enabled for this, they can push their code before merging to the default branch. 

And DB related changes can be done in the migration scripts and recreate the image in the pipeline.
