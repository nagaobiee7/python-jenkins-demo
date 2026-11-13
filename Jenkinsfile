
pipeline {
    agent {
        label 'ubuntu-python'
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
    }

    stages {
        stage('Checkout GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Verify Python') {
            steps {
                sh '''
                    whoami
                    hostname
                    python3 --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python -m pip install pytest build
                    .venv/bin/python -m pip install -e .
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '.venv/bin/python -m pytest -v'
            }
        }

        stage('Build Package') {
            steps {
                sh '.venv/bin/python -m build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/*',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Python CI/CD Build SUCCESS!'
        }

        failure {
            echo 'Python CI/CD Build FAILED!'
        }
    }
}
