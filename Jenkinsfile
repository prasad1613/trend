node{
      stage('build') {
            echo "-------------- build start --------------"
            sh "mvn clean deploy -Dmaven.test.skip=true"
            echo "--------------- build end --------------"
        }
      stage('unit test') {
         echo "-------------- build start --------------"
         sh "mvn surefire-report:report"
         echo "--------------- build end --------------"
      }
}
