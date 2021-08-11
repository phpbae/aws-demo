pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build..'
        sh './gradlew clean build'
      }
    }

    stage('upload') {
      steps {
        echo 'start war upload..'
        sh 'aws s3 cp build/libs/application.war s3://zero86/application.war --region us-east-2'
      }
    }

    stage('deploy') {
      steps {
        echo 'start deploy..'
        sh 'aws elasticbeanstalk create-application-version --region us-east-2 --application-name aws-demo --version-label ${BUILD_TAG} --source-bundle S3Bucket="zero86",S3Key="application.war"' 
        sh 'aws elasticbeanstalk update-environment --region us-east-2 --environment-name aws-demo-env-1 --version-label ${BUILD_TAG}'
      }
    }

  }
}
