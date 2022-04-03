/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    environment {
        EXIT_STATUS = 1
    }
    stages {
        stage('greeting') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
        stage('Build') {
            steps {
                echo 'building ...'
                script {
                    if (env.BRANCH_NAME == 'master') {
                        echo 'I only execute on the master branch'
                        sh 'sh test.sh'
                        EXIT_STATUS = 0
                    }
                    else {
                        echo "I execute elsewhere :  ${env.BRANCH_NAME}"
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing....'
                script {
                    if (EXIT_STATUS == 0) {
                        echo 'deploy !'
                    }
                    else
                    {
                        echo "do not deploy $EXIT_STATUS !!!"
                        sh 'exit $EXIT_STATUS'
                    }
                }
            }
        }
        stage('Parallel Stage') {
            when {
                branch 'master'
            }
            parallel {
                stage('Branch A') {
                    agent {
                        docker 'python:3'
                    }
                    steps {
                        echo 'hello world docker'
                    }
                }
                stage('Branch B') {
                    agent {
                        docker 'generatehtmlfromjson:v1.0'
                    }
                    steps {
                        echo 'local habt module to be pushed on git hub github'
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'send a success mail & update gerrit'
        }
        unstable { echo 'Build is unstable' }
        failure {
            echo 'send a fail mail'
        }
    }
}