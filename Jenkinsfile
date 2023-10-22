pipeline {
    agent {
        label 'docker-node'
    }
    environment {
        APP_NAME = "dmancloud/complete-prodcution-e2e-github-actions"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github-id', url: 'https://github.com/ayongpeterking/k8s-for-end-to-end.git'
            }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh '''
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                '''
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh '''
                    git config --global user.name "ayongpeterking"
                    git config --global user.email "ayongpeterking@yahoo.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                    withCredentials([gitUsernamePassword(credentialsId: 'github-id', gitRepoName: 'Default')]) {
                        sh 'git push https://github.com/ayongpeterking/k8s-for-end-to-end.git main'
                    }
                '''
            }
        }
    }
}
