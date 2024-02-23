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
                sh "mvn clean install deploy -Dmaven.test.skip=true"
                echo "----------------- build end -------------"
            }
        }
        stage('test') {
            steps {
                echo "--------------------- test start -------------"
                sh "mvn surefire-report:report"
                echo "--------------------- test end --------------"
            }
        }
    }
}