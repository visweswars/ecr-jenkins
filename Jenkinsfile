pipeline {
  agent any
  stages {
    stage ('GetEnv') {
      steps {
        sh 'printenv'
        
      }
    }
    stage ('Publish to ECR') {
      steps {
        withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/g0b5g9q2'
          sh 'docker build -t agency .'
          sh 'docker tag agency:latest public.ecr.aws/g0b5g9q2/agency:""$BUILD_ID""'
          sh 'docker push public.ecr.aws/g0b5g9q2/agency:""$BUILD_ID""'
        }
      }
    }
  }
}
