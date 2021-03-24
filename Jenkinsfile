#Jenkinsfile
node {
  stage('Preparation (Checking out)') 
    {
    echo 'Preparation...'
    //git branch: 'amerkushkin', url: 'https://github.com/AleksandrMerkushkin/mntlab-pipeline.git'
    git url: 'https://github.com/AleksandrMerkushkin/mntlab-pipeline.git'
    }
  stage('Building code')
    {
    echo 'Building...'    
    sh "/opt/gradle/bin/gradle build"
    }
  stage('Testing code')
    {
    echo 'Testing...'
    parallel (
      'Unit Tests': {sh '/opt/gradle/bin/gradle test' },
      'Jacoco Tests': {sh '/opt/gradle/bin/gradle jacocoTestReport' },
      'Cucumber Tests': {sh '/opt/gradle/bin/gradle cucumber' }
             )
    }
 stage('Triggering job and fetching artefact after finishing') 
    {
    echo 'Triggering...'
//   build job: 'MNTLAB-amerkushkin-child1-build-job', 
//   parameters: [string(name: 'BRANCH_NAME', value: "amerkushkin")]
    build job: "MNTLAB-amerkushkin-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "amerkushkin")], wait: true
        step ([$class: 'CopyArtifact',
          projectName: "MNTLAB-amerkushkin-child1-build-job",
          filter: '*.tar.gz']);
   //sh 'cp /var/lib/jenkins/workspace/MNTLAB-amerkushkin-child1-build-job/amerkushkin_dsl_script.tar.gz /var/lib/jenkins/workspace/Pipeline'
    }
  stage('Packaging and Publishing results') 
    {
    echo 'Packaging and Publishing results...'
   // sh 'cp /var/lib/jenkins/workspace/MNTLAB-amerkushkin-child1-build-job/amerkushkin_dsl_script.tar.gz /var/lib/jenkins/workspace/Pipeline'
    sh 'cd  /var/lib/jenkins/workspace/Pipeline'
    sh 'tar -xvf amerkushkin_dsl_script.tar.gz'
    sh 'tar -czf pipeline-amerkushkin-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
    archiveArtifacts "pipeline-amerkushkin-${BUILD_NUMBER}.tar.gz"
    }
  stage('Asking for manual approval')
    {
    //input “Ready to go??”
    input id: 'Answer', messauge: 'Deployment this artefact?', ok: 'Deploy'
    }
  stage('Deployment') 
    {
    echo 'Deploying...'
    sh 'java -jar build/libs/gradle-simple.jar'
    }
  stage('Sending status') 
   {
    echo "Gotovo!"
   }
  }
