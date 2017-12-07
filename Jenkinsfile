pipeline {
    
    agent { label 'WinSlave' }

    environment {
        def javaHome = tool 'JDK8';
        def scannerHome = tool 'sonarScanner';
        def mvnHome = tool 'Maven3';
        JAVA_HOME = "${javaHome}";
    }

    stages {
        stage('Test') {
            steps {
                bat "${mvnHome}\\bin\\mvn -B verify"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat '${scannerHome}\\bin\\sonar-scanner'
                }
            }
        }
        stage('Build') {
            steps {
                bat '${mvnHome}\\bin\\mvn -B -DskipTests clean package'
            }
        }
    }
}
