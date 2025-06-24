pipeline{
    agent{
        label 'agent-1'
    }
    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage('cleanup workspace'){
            cleanWs()
        }
        stage('checkout from scm'){
          git branch: 'main', credentialsId: 'github', url: 'https://github.com/GArunkumar999/register-app.git'

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
                   git config --global user.name "GArunkumar999"
                   git config --global user.email "giddaluruarunkumar9@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/GArunkumar999/gitops-register-app-cd.git main"
                }
            }
        }
    }
}
