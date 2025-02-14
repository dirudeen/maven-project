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
                    dir('/webpp/target/') {
                        stash includes: '*.war', name: 'maven-builds'
                    }
                }
            }

        }
        stage("deploy_dev") {
            when { expression { prams.ENVIRONMENT == 'dev'} 
            beforeAgent true}
            agent{
                label "devServer"
            }
            steps {
                dir('/var/www/html') {
                    unstash "maven-build"
                    sh """
                    pwd
                    cd /var/www/html/
                    jar -xvf webapp.war
                    """
                }
            }
        }
        stage("deploy_prod") {
            when { expression { prams.ENVIRONMENT == 'prod'} 
            beforeAgent true}
            agent{
                label "prodServer"
            }
            steps {
                dir('/var/www/html') {
                    unstash "maven-build"
                    sh """
                    pwd
                    cd /var/www/html/
                    jar -xvf webapp.war
                    """
                }
            }
        }
    }
}