
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
