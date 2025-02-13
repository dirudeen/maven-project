pipeline {
    agent {
        label 'devServer'
    }

    tools {
        maven 'mymaven'
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean package'
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