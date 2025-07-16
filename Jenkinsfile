pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker build -t debasishpanda5/django-todo:latest .'
                    sh 'docker push debasishpanda5/django-todo:latest'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('todo-frontend') {
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
                mail to: 'debasishpandayt@gmail.com',
                     subject: 'Approval needed for deployment',
                     body: 'Please approve the deployment of the project.'
                input message: 'Approve deployment?', ok: 'Yes'
            }
        }
        stage('Deploy') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }
}
