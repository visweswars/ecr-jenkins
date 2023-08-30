pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
        sh 'docker build -t viswareddy/jenkinsdemo:""$GIT_COMMIT"" .'
      }
    }
    
     stage ('Publish to DockerHub') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'docker push viswareddy/jenkinsdemo:""$GIT_COMMIT""'
         }
       }
     }
    stage ('Publish to ECR') {
      steps {
        //sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 890936820974.dkr.ecr.us-east-1.amazonaws.com'
        //withAWS(credentials: 'sam-jenkins-demo-credentials', region: 'eu-west-2') {
         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/t7e2c6o4' //890936820974.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr'
          sh 'docker build -t jenkins-ecr .'
          sh 'docker tag jenkins-ecr:latest 890936820974.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr:latest'
          sh 'docker push 890936820974.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr:latest'
         }
       }
    }
  }
}
