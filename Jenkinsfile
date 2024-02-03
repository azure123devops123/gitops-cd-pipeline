pipeline {
    agent {
        label 'dev'
    }
    
    environment {
        APPLICATION_NAME = "spring-first-app-jenkins-ci-pipeline"
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

        stage ('Update Deployment Tag') {
            steps {
                sh '''
                    cat deployment.yaml
                    sed -i 's/${APPLICATION_NAME}.*/${APPLICATION_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                '''
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