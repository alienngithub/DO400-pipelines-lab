pipeline {
    agent any
    parameters {
        booleanParam(name: "RUN_INTEGRATION_TESTS", defaultValue: true)
    }
    stages {
        stage('Test') {
            parallel {
                stage('Integration Tests') {
                    when {
                        expression { return params.RUN_INTEGRATION_TESTS}
                    }
                    steps {
                        sh 'mvn test -D testGroups=integration'
                    }
                }
                stage('Unit Tests') {
                    steps {
                        sh 'mvn test -D testGroups=unit'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    try {
                        sh 'mvn package -D skipTests'
                    } catch (ex) {
                        echo "Error while generating JAR file"
                        throw ex
                    }
                }
            }
        }
    }
}