@Library('my-shared-library') _

pipeline {

    agent any

    options {
        timestamps()
    }

    parameters {

        choice(
            name: 'action',
            choices: 'create\ndelete',
            description: 'Choose create/delete'
        )

        string(
            name: 'ImageName',
            description: 'Name of the docker image',
            defaultValue: 'javapp'
        )

        string(
            name: 'ImageTag',
            description: 'Tag of the docker image',
            defaultValue: 'v1'
        )

        string(
            name: 'DockerHubUser',
            description: 'DockerHub username',
            defaultValue: 'pvenki8890'
        )
    }

    stages {

        stage('Git Checkout') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                gitCheckout(
                    branch: "main",
                    url: "https://github.com/pvenki8890/Java_app_3.0.git"
                )
            }
        }

        stage('Unit Test Maven') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {
                    mvnTest()
                }
            }
        }

        stage('Integration Test Maven') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {
                    mvnIntegrationTest()
                }
            }
        }

        // SonarQube stages skipped due to infrastructure instability

        stage('Maven Build') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {
                    mvnBuild()
                }
            }
        }

        stage('Docker Image Build') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {

                    dockerBuild(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }

        // Trivy Scan skipped due to EC2 resource constraints

        stage('Docker Image Push : DockerHub') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {

                    dockerImagePush(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }

        stage('Docker Image Cleanup') {

            when {
                expression { params.action == 'create' }
            }

            steps {

                script {

                    dockerImageCleanup(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }
    }
}
