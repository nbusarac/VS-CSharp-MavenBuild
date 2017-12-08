pipeline {
    
    agent { label 'WinSlave' }

    environment {
        def javaHome = tool 'JDK8';
        def scannerHome = tool 'MSsonarScanner';
        def mvnHome = tool 'Maven3';
        JAVA_HOME = "${javaHome}";
    }

    stages {
        stage('Test') {
            steps {
                bat "${mvnHome}\\bin\\mvn dotnet:clean dotnet:compile dotnet:test"
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat "${scannerHome}\\bin\\sonar-scanner"
                }
            }
        }
        stage('Build') {
            steps {
                bat "${mvnHome}\\bin\\mvn dotnet:clean dotnet:compile dotnet:test install"
            }
        }
    }
}
