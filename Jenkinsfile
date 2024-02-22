environment {
        PATH = "/usr/share/maven/bin:$PATH"
    }
node{
      stage('build') {
            echo "-------------- build start --------------"
            sh "mvn clean deploy -Dmaven.test.skip=true"
            echo "--------------- build end --------------"

        }
}
