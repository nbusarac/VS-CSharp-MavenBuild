pipeline {
    
    agent { label 'WinSlave' }

    environment {
        def javaHome = tool 'JDK8';
        def scannerHome = tool 'MSsonarScanner';
        def mvnHome = tool 'Maven3';
        JAVA_HOME = "${javaHome}";
        def nugetHome = tool 'Nuget';
    }

    stages {
        stage('checkout') {
            bat "${mvnHome}\\bin\\mvn dotnet:clean dotnet:compile"
        }
        stage('Test') {
            steps {
                
                bat "${nugetHome}\\Nuget.exe rebuild *.sln"
                bat "C:\\Windows\\Microsoft.NET\\Framework64\\v4.0.30319\\MSBuild.exe /t:Rebuild"
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat "${scannerHome}\\bin\\SonarQube.Scanner.MSBuild.exe begin"
                    bat "${scannerHome}\\bin\\SonarQube.Scanner.MSBuild.exe begin"
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
