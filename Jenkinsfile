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
                script {
                    // Displaying the deployment.yaml contents before the update
                    echo '--- Before Update ---'
                    sh 'cat deployment.yaml'
                    echo '---------------------'

                    // Using the sh step to execute the sed command to update the nginx.yaml file
                    sh '''
                        sed -i "s|$APP_NAME|$APP_NAME:$IMAGE_TAG|g" deployment.yaml
                    '''

                    // Displaying the deployment.yaml contents after the update
                    echo '--- After Update ---'
                    sh 'cat deployment.yaml'
                    echo '--------------------'
                }
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    // Setting up Git configuration
                    sh '''
                        git config user.name "ayongpeterking"
                        git config user.email "ayongpeterking@yahoo.com"
                    '''

                    // Committing the changes
                    sh '''
                        git add deployment.yaml
                        git commit -m "Updated deployment.yaml with new image tag"
                    '''

                    // Pushing the changes back to the repository
                    // Assuming you have set up Git credentials in Jenkins with the ID 'github-credentials-id'
                    withCredentials([usernamePassword(credentialsId: 'github-id', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh '''
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/ayongpeterkin/k8s-for-end-to-end.git HEAD:main
                        '''}
             
                }
                
            }
        }
    }
}
