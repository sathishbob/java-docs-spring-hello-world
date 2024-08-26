pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
    }

    stages {
        stage('pull') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:sathishbob/java-docs-spring-hello-world.git'
            }
        }
        
        stage('build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean install"
            }
        }
        
        stage('publish') {
            steps {
                junit 'target/surefire-reports/*.xml'
                archiveArtifacts 'target/*.jar'
            }
            post {
                success {
                    emailext body: "Please check the console output at $BUILD_URL for more information", to: "sathishbabudevops@gmail.com", subject: '$PROJECT_NAME is completed - Build number is $BUILD_NUMBER and build status is $BUILD_STATUS'
                }
            }
        }
        stage('command execution') {
            steps {
                sh 'ls test.txt'
            }
            post {
                failure {
                    emailext body: "Please check the console output at $BUILD_URL for more information", to: "sathishbabudevops@gmail.com", subject: '$PROJECT_NAME is failled - Build number is $BUILD_NUMBER and build status is $BUILD_STATUS'
                }
            }
        }
    }
}
