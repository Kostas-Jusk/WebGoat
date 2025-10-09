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

        // stage('Build') {
        //     steps {
        //         bat '''
        //             set JAVA_HOME=%JAVA_HOME%
        //             set PATH=%JAVA_HOME%\\bin;%PATH%
        //             call "%MAVEN_HOME%\\bin\\mvn" clean compile -DskipTests
        //         '''
        //     }
        // }

        // stage('Unit Test') {
        //     steps {
        //         bat '''
        //             set JAVA_HOME=%JAVA_HOME%
        //             set PATH=%JAVA_HOME%\\bin;%PATH%
        //             call "%MAVEN_HOME%\\bin\\mvn" test
        //         '''
        //     }
        // }

        // stage('Integration Test') {
        //     steps {
        //         bat '''
        //             set JAVA_HOME=%JAVA_HOME%
        //             set PATH=%JAVA_HOME%\\bin;%PATH%
        //             call "%MAVEN_HOME%\\bin\\mvn" verify -DskipUnitTests=true
        //         '''
        //     }
        // }

        // stage('Static Analysis') {
        //     steps {
        //         withSonarQubeEnv('local-sonarqube') {
        //             bat '''
        //                 set JAVA_HOME=%JAVA_HOME%
        //                 set PATH=%JAVA_HOME%\\bin;%PATH%
        //                 call "%MAVEN_HOME%\\bin\\mvn" sonar:sonar ^
        //                 -Dsonar.projectKey=Kostas-Jusk_WebGoat ^
        //                 -Dsonar.host.url=%SONAR_HOST_URL%
        //             '''
        //         }
        //     }
        // }

        // stage('Package') {
        //     steps {
        //         bat '''
        //             set JAVA_HOME=%JAVA_HOME%
        //             set PATH=%JAVA_HOME%\\bin;%PATH%
        //             call "%MAVEN_HOME%\\bin\\mvn" package -DskipTests
        //         '''
        //     }
        // }

        stage('Dependency Updates (Renovate)') {
            steps {
                withCredentials([string(credentialsId: '72423783-530c-4723-8815-ec9f0461e727', variable: 'RENOVATE_TOKEN')]) {
                    bat '''
                        set "NODE_HOME=%WORKSPACE%\\..\\..\\..\\..\\node-v22.20.0-win-x64"
                        set "PATH=%NODE_HOME%;%WORKSPACE%\\node_modules\\npm\\bin;%PATH%"
                        set "npm_config_cache=C:\\Users\\kostas.juskevicius\\istg_cant_run_anything_on_this_pc_due_to_security\\.npm_cache"

                        "%NODE_HOME%\\npm.cmd" install -g renovate

                        npx renovate --require-config=false --platform=github --token=%RENOVATE_TOKEN% --log-level=debug > renovate.log 2>&1
                        
                        echo "idk anymore"
                        type renovate.log
                        npx renovate --version
                        npx renovate --help
                    '''
                }
            }
        }
    }
}
