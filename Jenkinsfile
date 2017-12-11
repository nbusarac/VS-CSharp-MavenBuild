pipeline {
    
    agent { label 'WinSlave' }

    environment {
        def javaHome = tool 'JDK8';
        def scannerHome = tool 'sonarScanner';
        def mvnHome = tool 'Maven3';
        def nugetHome = 'C:\\Jenkins\\tools\\nuget';
        JAVA_HOME = "${javaHome}";
    }

    stages {
        stage('Test') {
            steps {
                bat "${nugetHome}\\nuget.exe restore .\\UnitTestProject1\\"
                bat "${mvnHome}\\bin\\mvn dotnet:clean dotnet:build dotnet:test"
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
                bat "${mvnHome}\\bin\\mvn dotnet:clean dotnet:build install"
            }
        }
    }
}
