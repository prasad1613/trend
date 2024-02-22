pipeline {
    agent any
    environment {
        PATH = "/usr/share/maven/bin:$PATH"
    }
    stages {
        stage('build') {
            echo "-------------- build start --------------"
            sh "mvn clean deploy -Dmaven.test.skip=true"
            echo "--------------- build end --------------"

        }
    }
}