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
   
    // Put your stages here
  }
}
stage('Detect build type') {
  steps {
    script {
      if (env.BRANCH_NAME == 'develop' || env.CHANGE_TARGET == 'develop') {
        env.BUILD_TYPE = 'debug'
      } else if (env.BRANCH_NAME == 'master' || env.CHANGE_TARGET == 'master') {
        env.BUILD_TYPE = 'release'
      }
    }
  }
}
stage('Compile') {
  steps {
    // Compile the app and its dependencies
    sh './gradlew compile${BUILD_TYPE}Sources'
  }
}
stage('Build') {
  steps {
    // Compile the app and its dependencies
    sh './gradlew assemble${BUILD_TYPE}'
    sh './gradlew generatePomFileForLibraryPublication'
  }
}
stage('Publish') {
  steps {
    // Archive the APKs so that they can be downloaded from Jenkins
    archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.apk"
    // Archive the ARR and POM so that they can be downloaded from Jenkins
    // archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.aar, **/*pom-   default.xml*"
  }
}