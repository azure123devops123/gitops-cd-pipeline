def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline {
    agent {
        label 'dev'
    }
    
    environment {
        APPLICATION_NAME = "java-spring-app-ci-pipeline"

        GITHUB_USERNAME = "devopstech24"
        EMAIL = "azure123.devops123@gmail.com"
    }

    stages {
        stage ('Workspace Cleanup') {
            steps {
                cleanWs()
            }  
        }

        stage ('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/azure123devops123/gitops-cd-pipeline'
            }  
        }

        stage ('Update Deployment Tag') {
            steps {
                // always use double quotes below otherwise it will not work properly
                // Stream EDitor (sed) => sed -i 's/old-text/new-text/g' input.txt
                sh """
                    cat deployment.yaml
                    sed -i 's/${APPLICATION_NAME}.*/${APPLICATION_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage ('Push the changed Deployment file to Github Repository') {
            steps {
                // always use double quotes below otherwise it will not work properly
                // Stream EDitor (sed) => sed -i 's/old-text/new-text/g' input.txt
                sh """
                    git config --global user.name '${GITHUB_USERNAME}'
                    git config --global user.email "${EMAIL}"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                // Use Snippet Generator for code => withCredentials
                withCredentials([gitUsernamePassword(credentialsId: 'github-cred', gitToolName: 'Default')]) {
                    sh "git push https://github.com/azure123devops123/gitops-cd-pipeline main"
                }
            }
        }

    } // end of stages section

    post {
        always {
            echo 'Slack Notifications'
            slackSend (
                channel: '#spring-app',   // change your channel name
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} \n build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
            )
        }
    }
    
    // post {
    //     failure {
    //         emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
    //                 subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
    //                 mimeType: 'text/html',to: "azure123.devops123@gmail.com"
    //         }
    //      success {
    //            emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
    //                 subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
    //                 mimeType: 'text/html',to: "azure123.devops123@gmail.com"
    //       }      
    // }

}