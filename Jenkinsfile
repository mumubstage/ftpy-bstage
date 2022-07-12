def JOB_NAME_PARTS = JOB_NAME.tokenize('/') as String[]
def JOB_CONSOLE_NAME = JOB_NAME_PARTS[0]
def JOB_NAMESPACE_NAME = JOB_NAME_PARTS[1]
def JOB_BRANCH_NAME = JOB_NAME_PARTS[2]
def BRANCH_MASTER = 'main'

pipeline {
    agent {
        node {
            label 'runner'
        }
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    sh """
                    python3.8 -m virtualenv venv
                    . venv/bin/activate
                    python3.8 -m pip install -r requirements-dev.txt
                    """
                }
            }
        }

        stage('Unit tests - Python') {
            steps {
                sh """
                . venv/bin/activate
                PYTHONPATH=. pytest
                """
            }
        }

        stage('Code Analysis (SonarQube)') {
            environment {
                scannerHome = tool 'SonarQube Scanner'
            }
            steps {
                withSonarQubeEnv('SonarQube Server') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=credentials('sonar-login') -Dproject.settings=sonar-project.properties"
                }
            }
        }   
    }
}
