
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
             git branch: 'main', url: 'https://github.com/ooghenekaro/argocd-manifest.git'  
           }
        }  
        stage('Update Deployment manifest') {
               environment {
                         GIT_REPO_NAME="argocd-manifest"
                         APP_NAME="nodejs-app"
                         GIT_USERNAME="ooghenekaro"
                 
             }
            steps {
                    withCredentials([usernamePassword(credentialsId: 'karosec-github', variable: 'GIT_TOKEN')]) {   
                        sh "git config user.email ooghenekaro@yahoo.com"
                        sh "git config user.name ooghenekaro"
                        sh "cat deployment.yml"
                        sh "sed -i 's+${APP_NAME}.*+${APP_NAME}:${DOCKERTAG}+g' deployment.yml"
                        sh "cat deployment.yml"
                        sh "git add ."
                        sh "git commit -m 'Update the Deployment image to this version: ${BUILD_NUMBER}'"
                        sh "git push https://${GIT_TOKEN}@github.com/${GIT_USERNAME}/${GIT_REPO_NAME} HEAD:main" 
                    }
           }
       }
    }
}
