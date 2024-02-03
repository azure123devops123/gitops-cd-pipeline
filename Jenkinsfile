pipeline {
    agent {
        label 'dev'
    }

    stages {
        stage ('Workspace Cleanup') {
            steps {
                cleanWs()
            }  
        }

        stage ('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/azure123devops123/gitops-cd-pipeline'
            }  
        }

        stage ('Demo Image Tag') {
            steps {
                echo '${IMAGE_TAG}'
            }
        }

    } // end of stages

    post {
        failure {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                    mimeType: 'text/html',to: "azure123.devops123@gmail.com"
            }
         success {
               emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                    mimeType: 'text/html',to: "azure123.devops123@gmail.com"
          }      
    }

}