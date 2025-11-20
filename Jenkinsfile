pipeline {

    agent any

    parameters {
        choice(name: 'select_environment', choices: ['dev', 'prod'])
    }

    environment {
        NAME = "piyush"
    }

    tools {
        maven 'maven-3.9.11'
    }

    stages {

        stage('Build') {
            steps {
                script {
                    def file = load "script.groovy"
                    file.hello()
                }
                sh "mvn clean package -DskipTests=true"
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh "mvn test"
            }

            post {
                success {
                    dir("webapp/target") {
                        stash name: "maven-build", includes: "*.war"
                    }
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                    cd /var/www/html
                    jar -xvf webapp.war
                """
            }
        }
    }
}
