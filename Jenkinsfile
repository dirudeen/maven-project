pipeline {
    agent {
        label 'devServer'
    }

    parameters {
        choice choices: ['dev', 'prod'], description: 'Choice to select which environment to run on', name: 'ENVIRONMENT'
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
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage("Test") {
            parallel {
                stage("test A") {
                    agent{
                        label 'devServer'
                    }
                    steps {
                        echo "this is the test A"
                        sh 'mvn test'
                    }

                }
                stage("test B") {
                    agent {
                        label 'devServer'
                    }
                    steps {
                        echo "this is the test B"
                        sh 'mvn test'
                    }

                }

            }

            post {
                success {
                    dir('webapp/target/') {
                        stash includes: "*.war", name: 'maven-build'
                    }
                }
            }

        }
        stage("deploy_dev") {
            when { expression { params.ENVIRONMENT == 'dev'} 
            beforeAgent true}
            agent{
                label "devServer"
            }
            steps {
                dir('/tmp/deploy') {  // Use a temp folder to avoid permission issues
                    unstash "maven-build"
                    sh """
                    sudo mv webapp.war /var/www/html/
                    cd /var/www/html/
                    sudo jar -xvf webapp.war
                    """
                }
            }

        }
        stage("deploy_prod") {
            when { expression { params.ENVIRONMENT == 'prod'} 
            beforeAgent true}
            agent{
                label "prodServer"
            }
            steps {
                dir('/tmp/deploy') {
                    unstash "maven-build"
                    sh """
                    sudo mv webapp.war /var/www/html/
                    cd /var/www/html/
                    sudo jar -xvf webapp.war
                    """
                }
            }
        }
    }
}