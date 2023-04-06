pipeline {
    agent any
    environment {
        APP_NAME = "ooghenekaro/nodejs-app"
        
        stages {
             stage('Clone repository') {
                 steps{
             git branch: 'main', url: 'https://github.com/ooghenekaro/argocd-manifest.git'  
                  }
             }
        stage('Updating Kubernetes deployment file'){
            steps {
                sh "cat deployment.yml"
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${DOCKERTAG}/g' deployment.yml"
                sh "cat deployment.yml"
            }
        }
        stage('Push the changed deployment file to Git'){
            steps {
                script{
                    sh """
                    git config --global user.name "ooghenekaro"
                    git config --global user.email "ooghenekaro@yahoo.com"
                    git add deployment.yml
                    git commit -m 'Updated the deployment file' """
                    withCredentials([usernamePassword(credentialsId: 'karo-github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh "git push http://$user:$pass@github.com/ooghenekaro/argocd-manifest.git main"
                    }
                }
            }
        }
    }
}
