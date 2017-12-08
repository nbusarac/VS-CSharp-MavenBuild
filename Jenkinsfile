pipeline {
    
    agent { label 'WinSlave' }

    environment {
        def javaHome = tool 'JDK8';
        def scannerHome = tool 'MSsonarScanner';
        def mvnHome = tool 'Maven3';
        JAVA_HOME = "${javaHome}";
        def nugetHome = "c:\\Jenkins\\tools\\nuget";
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
                    bat "${nugetHome}\\nuget.exe restore"
                    bat "\"C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\MSBuild\\15.0\\Bin\\MSBuild.exe\" /t:Rebuild"
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
