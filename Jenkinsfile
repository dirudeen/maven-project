pipeline {
    agent any

    parameters {
        string defaultValue: 'Colley', name: 'LASTNAME'
    }

    tools {
        maven 'mymaven'
    }
    
    environment {
        NAME = "Hadiru"
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean package'
                echo "hello $NAME ${params.LASTNAME}"
            }
        }

        stage("Test") {
            parallel {
                stage("test A") {
                    steps {
                        echo "this is the test A parallel stage"
                    }

                }
                stage("test B") {
                    steps {
                        echo "this is the test B parallel stage"
                    }

                }

            }

            post {
                success {
                   archiveArtifacts artifacts: '**/target/*.war', followSymlinks: false, onlyIfSuccessful: true
                }
            }

        }
        // stage("Deploy") {
        //     steps {
        //         echo "this is the Deployment stage"
        //     }
        // }
    }
}