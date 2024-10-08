pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app-ci"
    }
    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Image tag for the deployment')
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/Code-Cafe-IT/gitops-register-app-ytb.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Code-Cafe-IT"
                   git config --global user.email "laminhduc2k1@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Code-Cafe-IT/gitops-register-app-ytb.git main"
                }
            }
        }
      
    }
}
