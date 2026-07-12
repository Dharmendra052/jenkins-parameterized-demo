pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
        timestamps()
    }

    parameters {

        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'QA', 'PROD'],
            description: 'Select deployment environment'
        )

        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Application Version'
        )

        string(
            name: 'BRANCH',
            defaultValue: 'main',
            description: 'Git Branch'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run Unit Tests'
        )

    }

    environment {
        APPLICATION = "jenkins-parameterized-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: params.BRANCH,
                    credentialsId: 'github-ssh',
                    url: 'git@github.com:Dharmendra052/jenkins-parameterized-demo.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${env.APPLICATION}"
                sh 'echo Build Successful'
            }
        }

        stage('Unit Test') {
            when {
                expression { params.RUN_TESTS }
            }

            steps {
                echo "Running Unit Tests..."
                sh 'echo All Tests Passed'
            }
        }

        stage('Approval') {

            when {
                expression {
                    params.ENVIRONMENT == 'PROD'
                }
            }

            steps {
                input(
                    message: "Deploy Version ${params.VERSION} to Production?",
                    ok: "Approve"
                )
            }
        }

        stage('Deploy') {

            steps {

                script {

                    if (params.ENVIRONMENT == "DEV") {

                        sh """
                        echo "Deploying Version ${params.VERSION} to DEV"
                        """

                    } else if (params.ENVIRONMENT == "QA") {

                        sh """
                        echo "Deploying Version ${params.VERSION} to QA"
                        """

                    } else {

                        sh """
                        echo "Deploying Version ${params.VERSION} to PROD"
                        """

                    }

                }

            }

        }

    }

    post {

        always {
            echo "Pipeline Finished"
        }

        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }

    }

}
