pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "cultigestapp"

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/DevopsEasy/gitops-cultigestapp.git'
            }

        }
        stage("Update the deployment file"){
            steps {

                sh """
                 cat deployment.yaml
                 sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                 cat deployment.yaml

                 """
            }
        }
         stage("push the changes"){
            steps {
                sh """
                 git config --global user.name "devopseasy"
                 git config --global user.email "devopseasy@gmail.com"
                 git add deployment.yaml
                 git commit -m "updated manifest"
                 
                 """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                 sh "git push https://github.com/DevopsEasy/gitops-cultigestapp main"
                }
            }
        }

        }
}
