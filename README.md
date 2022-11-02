# COSC2759 Assignment 1
## Notes App - CI Pipeline
- Full Name: Matthew Paul Bird
- Student ID: s3482450


## 1. CI pipeline (ci-unit testing, ci-integration testing, -ci e2e testing and package)
### 1.1 analysis of the problem:
The problem is that Alpine Inc is relying upon manual testing (previously performed by Pete the Senior Dev). This means that the entire development process is dependant on the presence of Pete, and that operations can only work well with the presence of Pete. The application is subject to many potential errors. During the development process, these errors can be introduced in a multitude of ways. 

The first being that there is no static code Anaylsis or automated unit testing. This means that small bugs within the program can easily slip through to deployment. 


The second problem is that there is no integration testing. This means that there is no validation that the connection to the  MongoDb backend. 

The third problem is that there is no end to end testing. 

The fourth problem is that packaging of the program can be performed without the vailidation of the above menitoned problems have been checked. 

### 1.2 soloution
The solution to the problem is to introduce a CI pipeline to perform these automated tests:

first, a Lint check will be used to perform a static code analysis. This will also include automatic unit tests. 

secondly, if the above test passes, an automatic integration test will be performed to validate that the connection to the mongodb is viable. This will use a docker container for the DB.

Thirdly, if the two tests above pass, end to end testing will be performed to vailidate that the user interface is working correctly. If this also passes then it can be assumed that the application is viable for deployment. 

Finally, providing all checks have passed AND the application has been merged onto the main branch (ready for deployment), the application will package. 


### 1.3 soloution as a diagram: 

<img src="/img/Ci-pipeline.png" style="height: 800px; width: 600px"/>


## 2. failure scenarios and final test
### 2.1  demo of full pipeline working (without merge onto main): workflow pipeline #162
here the full pipeline runs, minus the artefact from the packaging job, as it is not on the main branch. 
<img src="/img/demoNoMain.png" style="height: 540px; width: 960px"/>

### 2.2 Implement a failure scenario- lint: workflow pipeline #170
Here the pipeline will break on the lint test, due to a bug introduced into the notes.js file:

<img src="/img/lintBug.png" style="height: 540px; width: 960px"/>

<img src="/img/LintError.png" style="height: 540px; width: 960px"/>

<img src="/img/pipeBreakLint.png" style="height: 540px; width: 960px"/>

Lint has failed to pass and the build has not progressed. 

### 2.2 Implement a failure scenario- unit test:   workflow pipeline #172

Here a unit test is altered to fail. : 
<img src="/img/unitTestBug.png" style="height: 540px; width: 960px"/>

<img src="/img/UnitError.png" style="height: 540px; width: 960px"/>

<img src="/img/UnitError.png" style="height: 540px; width: 960px"/>


### 2.3 Implement a failure scenario- integration test:  workflow pipeline #178

Here the expected status code of the integration test was changed from 302 to 0 to make the test fail: 

<img src="/img/integrationFailed.png" style="height: 540px; width: 960px"/>

<img src="/img/integrationPipeFail.png" style="height: 540px; width: 960px"/>


The integration test failed to pass and the build has not progressed. 

### 2.4 Implement a failure scenario- e2e test: workflow pipeline #177
Here the webserver in playwright.config.ts was commented out, causing the end to end testing to fail: 

<img src="/img/e2eFail.png" style="height: 540px; width: 960px"/>

<img src="/img/e2eFailPipe.png" style="height: 540px; width: 960px"/>

The integration test failed to pass and the build has not progressed

### 2.5  demo of full pipeline working (with merge onto main): workflow pipeline #182

Here the final changes were made and then merged onto main, resulting in a full pipe run and package artefact: 

<img src="/img/fullPipePackage.png" style="height: 540px; width: 960px"/>


## 2. other notes
### 2.1  e2e development on incorrect branch:

Some parts of the e2e testing were developed on an incorrect branch. There is a comment highlighting this. I had originally been unable to get e2e testing to work on github actions and had prepared to move onto finishing the "implement failure scenarios" and readme. I had an idea the next day on how to fix it, which seems to have worked. However part of it was done on a branch "feature/failureScenarioTests". This was the incorrect branch to perform this development on. I then addressed this by continuing development on "features/e2eArtifact".