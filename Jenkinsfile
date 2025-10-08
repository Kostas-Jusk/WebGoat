pipeline {
    agent { label 'agent-1' }

    environment {
        MAVEN_HOME = tool 'maven'
        SONARQUBE_SCANNER_HOME = tool 'sonar-scanner'
        JAVA_HOME = tool 'jdk-25'
        SONARQUBE_ENV = 'local-sonarqube'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Kostas-Jusk/WebGoat', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                bat '''
                    set JAVA_HOME=%JAVA_HOME%
                    set PATH=%JAVA_HOME%\\bin;%PATH%
                    call "%MAVEN_HOME%\\bin\\mvn" clean compile -DskipTests
                '''
            }
        }

        stage('Unit Test') {
            steps {
                bat '''
                    set JAVA_HOME=%JAVA_HOME%
                    set PATH=%JAVA_HOME%\\bin;%PATH%
                    call "%MAVEN_HOME%\\bin\\mvn" test
                '''
            }
        }

        stage('Integration Test') {
            steps {
                bat '''
                    set JAVA_HOME=%JAVA_HOME%
                    set PATH=%JAVA_HOME%\\bin;%PATH%
                    call "%MAVEN_HOME%\\bin\\mvn" verify -DskipUnitTests=true
                '''
            }
        }

        stage('Static Analysis') {
            steps {
                withSonarQubeEnv('local-sonarqube') {
                    bat '''
                        %SONARQUBE_SCANNER_HOME%\\bin\\sonar-scanner.bat ^
                          -Dsonar.projectKey=Kostas-Jusk_WebGoat ^
                          -Dsonar.sources=src/main/java ^
                          -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }

        stage('Package') {
            steps {
                bat '''
                    set JAVA_HOME=%JAVA_HOME%
                    set PATH=%JAVA_HOME%\\bin;%PATH%
                    call "%MAVEN_HOME%\\bin\\mvn" package -DskipTests
                '''
            }
        }
    }
}
