pipeline {
  agent any
  environment {
    APP_NAME = 'test'
  }
  options {
    // Stop the build early in case of compile or test failures
    skipStagesAfterUnstable()
  }
  stages {
   stage('Detect build type') {
  steps {
    script {
      if (env.BRANCH_NAME == 'main' || env.CHANGE_TARGET == 'develop') {
        env.BUILD_TYPE = 'debug'
      } else if (env.BRANCH_NAME == 'main' || env.CHANGE_TARGET == 'main') {
        env.BUILD_TYPE = 'release'
      }
    }
  }
}
stage('Compile') {
  steps {
    // Compile the app and its dependencies
    sh "chmod 777 ./gradlew"
    //sh "echo <password> | sudo -S <cmd>" 
    sh "./gradlew compiledebugSources"
  }
}
    
    stage('Build') {
  steps {
    // Compile the app and its dependencies
    sh "./gradlew assemble${BUILD_TYPE}"
    sh './gradlew generatePomFileForLibraryPublication'
  }
}

stage('Publish') {
  steps {
     //Archive the APKs so that they can be downloaded from Jenkins
     archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.apk"
     //Archive the ARR and POM so that they can be downloaded from Jenkins
     archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.aar, **/*pom-   default.xml*"
  }
}
    // Put your stages here
  }
}
