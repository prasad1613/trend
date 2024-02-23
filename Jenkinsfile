pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment {
    PATH = "/opt/maven/bin:$PATH"
}
    stages {
        stage('build') {
            steps {
                echo "---------------- build start -------------"
                sh "mvn clean install deploy"
                echo "----------------- build end -------------"
            }
        }
    }
}