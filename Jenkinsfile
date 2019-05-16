pipeline {
  agent any
  stages {
    stage('Build') {
      when {
        beforeAgent true
        branch 'development'
      }
      steps {
        echo 'Build Step in Build Stage'
        writeFile file: "application.sh", text: "echo Built ${BUILD_ID} of ${JOB_NAME}"
        archiveArtifacts 'application.sh'
        gateProducesArtifact file: 'application.sh', label: 'Dummy artifact to be consumed by Deploy (master branch) gate'
      }
    }
    stage('Test') {
      when {
        beforeAgent true
        branch 'test'
      }
      steps {
        error 'fake error to force failure in test stage/gate'
        copyArtifacts projectName: '../helloworld-api/development'
        gateConsumesArtifact file: 'application.sh'
      }
    }
    stage('Deploy') {
      when {
        beforeAgent true
        branch 'master'
      }
      steps {
        echo "Deploy step in Deploy stage"
        copyArtifacts projectName: '../helloworld-api/development'
        gateConsumesArtifact file: 'application.sh'
      }
    }
  }
}
