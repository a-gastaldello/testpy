pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: python
                image: python:3.11-alpine
                tty: true
            '''
            defaultContainer 'python'
            retries 2
        }
    }
    stages {
        stage('Install Poetry') {
            steps {
                sh 'pip install poetry'
                sh ' poetry config virtualenvs.create false'
                sh 'poetry install'
            }
        }
        stage('Run tests') {
            steps {
                sh 'poetry run pytest'
            }
        }
        state('Lint') {
            steps {
                sh 'poetry run black src'
            }
        }
    }
}
