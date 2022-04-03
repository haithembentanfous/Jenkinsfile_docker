pipeline {
    agent any
    environment { 
        EXIT_STATUS = 1
    }
    stages {
        stage('greeting') {
            steps{
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'echo "exit status $EXIT_STATUS"'
            }
        }
        stage('Build') {
            steps{
                echo 'building ...'
                script {
                    if (env.BRANCH_NAME == 'master') {
                        echo 'I only execute on the master branch'
                        sh 'sh test.sh'
                        EXIT_STATUS = 1

                    } else {
                        echo "I execute elsewhere :  ${env.BRANCH_NAME}"
                    }
                }
            }
        }
        stage('Test') {
            steps{
                echo 'Testing....'
                script{
                    if (EXIT_STATUS == 0) {
                        echo 'deploy !'
                    }
                    else
                    {
                        echo "do not deploy $EXIT_STATUS !!!"
                        sh 'exit "${EXIT_STATUS}"'
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