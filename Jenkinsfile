pipeline{
    agent{
        label 'agent-1'
    }
    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage('cleanup workspace'){
            steps{
                 cleanWs()
             }
        }
        stage('checkout from scm'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/GArunkumar999/gitops-register-app-cd.git'

            }

        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                    sed -i 's|image: .*|image: arun596/registerapp:${IMAGE_TAG}|g' deployment.yaml

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
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    sh """
                       git remote set-url origin https://${GIT_USER}:${GIT_TOKEN}@github.com/GArunkumar999/gitops-register-app-cd.git
                       git push origin main
                    """
                }
                
            }
        }
    }
}
