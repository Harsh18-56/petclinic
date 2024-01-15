pipeline{
  agent any
  tools {
        maven "maven3"
    }  
    stages{
        stage('Git Scm'){
           steps{
               git credentialsId: 'github_creden', url: 'https://github.com/Harsh18-56/petclinic.git'
           }
        }
    stage('marchieve artifacts'){
           steps{
               archiveArtifacts artifacts: '**', followSymlinks: false
           }
        }
    stage('deploy'){
           steps{
               withCredentials([usernameColonPassword(credentialsId: 'tomcat_creden', variable: '')]) {
                 sh "curl -v -u harsh:harsh -T /var/lib/jenkins/workspace/petclinic-pipeline 'http://3.110.196.93:8181/manager/text/deploy?path=/pipeline_java1'" 
                 }
                }
            }
        }
    
       post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "harshkamble200002@gmail.com",
                sendToIndividuals: true])
        }
      environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "13.200.215.96:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "petclinic1"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus_crden"
    }
    stages {
        stage("clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/Harsh18-56/petclinic.git';
                }
            }
        }
        stage("mvn build") {
            steps {
                script {
                      sh "mvn package"
        }
            }
        }
       }
    }
