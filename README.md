# Jenkins-pipelines
Task 1. Automated Deployments
Review
The pipeline will be created by automated way on public Jenkins http://jenkins-mntlab.dynu.com/ by using Scripted Jenkinsfile
Task
1)	Install Pipeline plugin (workflow-aggregator) for the local Jenkins.
2)	Configure Jenkins tools according to APPENDIX B. (new plugin installing might be required)
3)	Create Jenkinsfile for automated creation the following Jenkins jobs: 1 main and 4 child. 
4)	The Scripted Jenkinsfile are stored in github repo https://github.com/bubalush/mntlab-pipeline (root location in repo), Each student commits all step-by-step changes in own branch named {student}. Initial source = ‘master’ branch

Where {student} is a first char from name + surname (all in lower case). Example: Aleksandr Mazurenko  = amazurenko

5)	Requirements for the Pipeline:
- Please see APPENDIX A.
- Details of stages:

1) For the remote Jenkins execution should be restricted on ‘host’ node.
Locally it can be done where student wants
2) ‘Preparation (Checking out)’
Checkout code from {student} repo
3) ‘Building code’
 Should contain building code stage
Additional commands:
gradlew wrapper for execution of gradle commands.
 			     build - instruction for gradle to execute build task
 
4)  ‘Testing code’
Should contain 3 parallel execution of tests
     Stages:  ‘Unit Tests’, ‘Jacoco Tests’, ‘Cucumber Tests’
Additional commands:
            gradlew wrapper for execution of gradle commands.
  test  - run Unit tests
  jacoco - run Jacoco tests
            cucumber - run Cucumber tests

5)  ‘Triggering job and fetching artefact after finishing’
On this stage the job MNTLAB-{student}-child1-job should be triggered with {student} branch parameter. (the job from previous DSL lab). Job should fetch code from ‘MNT-Lab/mntlab-dsl’ from {student} branch.
- Pipeline should wait for finishing triggered job, take created artefact.

6) ‘Packaging and Publishing results’
On this stage the job should:
a) take:
 -  ‘jobs.groovy’ file (from child1-job archive), 
- ‘Jenkinsfile’, 
- built ‘gradle-simple.jar’ 

b) Should create new artefact ‘pipeline-{student}-{buildNumber}.tar.gz’ (where buildNumber - number of the current build)
   c) Should attach this artefact to current job.

7) ‘Asking for manual approval’
Once previous stage successful the job should ask for deployment this artefact.

8) 'Deployment'
Artefact should be deployed ;
Use command ‘java -jar gradle-simple.jar’

9) ‘Sending status’
The message about SUCCESS should be sent to output.
