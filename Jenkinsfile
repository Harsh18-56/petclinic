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
       }
    }
