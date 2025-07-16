pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                dir('todo_project') {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker build -t debasishpanda5/django-todo:latest .'
                        sh 'docker push debasishpanda5/django-todo:latest'
                    }
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('todo_project/todo-frontend') {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker build -t debasishpanda5/react-todo:latest .'
                        sh 'docker push debasishpanda5/react-todo:latest'
                    }
                }
            }
        }
        stage('Approval') {
            steps {
                script {
                    def buildUrl = env.BUILD_URL
                    def approvalUrl = "${buildUrl}input/${executedBuild.inputProceed}"
                    mail to: 'debasishpandayt@gmail.com',
                         subject: 'Approval needed for deployment',
                         mimeType: 'text/html',
                         body: """
                             <html>
                             <body>
                             <p>Please approve or reject the deployment by clicking one of the links below:</p>
                             <p><a href="${approvalUrl}&proceed=true">Yes</a> | <a href="${approvalUrl}&proceed=false">No</a></p>
                             <p>Alternatively, approve or reject manually at: <a href="${buildUrl}input/">${buildUrl}input/</a></p>
                             <p>Build URL: <a href="${buildUrl}">${buildUrl}</a></p>
                             </body>
                             </html>
                         """
                    input message: 'Approve deployment?', ok: 'Yes'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }
}
