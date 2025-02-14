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
                echo 'hello $NAME ${params.LASTNAME}'
            }

            post {
                success {
                   archiveArtifacts artifacts: '**/target/*.war', followSymlinks: false, onlyIfSuccessful: true
                }
            }

        }
        // stage("Test") {
        //     steps {
        //         echo "this is the test stage"
        //     }
        // }
        // stage("Deploy") {
        //     steps {
        //         echo "this is the Deployment stage"
        //     }
        // }
    }
}