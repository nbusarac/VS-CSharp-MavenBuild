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
                    bat "${scannerHome}\\SonarQube.Scanner.MSBuild.exe begin /k:\"org.sonarqube:sonarqube-scanner-msbuild\" /n:\"Example of SonarQube Scanner for MSBuild Usage\" /v:\"1.0\""
                    bat 'C:\\Windows\\Microsoft.NET\\Framework64\\v4.0.30319\\MSBuild.exe /t:Rebuild'
                    bat "${scannerHome}\\SonarQube.Scanner.MSBuild.exe end"
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
