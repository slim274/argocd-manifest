
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
             }
            steps {
                    withCredentials([usernamePassword(credentialsId: 'karosec-github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {   
                        sh "git config user.email ooghenekaro@yahoo.com"
                        sh "git config user.name ooghenekaro"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+ooghenekaro/nodejs-app.*+ooghenekaro/nodejs-app:${BUILD_NUMBER}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Update the Deployment image to this version: ${BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/${GIT_REPO_NAME} HEAD:main"
               }
           }
       }
    }
}
