pipeline {
  agent any
  triggers {
    pollSCM '* * * * *'
  }
  stages {
   stage('Docker build and push') {
      steps {
        sh '''
          whoami
          echo $access_key
         aws configure set aws_access_key_id $access_key
         aws configure set aws_secret_access_key $secret_key
         aws configure set default.region ap-south-1
         DOCKER_LOGIN_PASSWORD=$(aws ecr get-login-password  --region ap-south-1)
         docker login -u AWS -p $DOCKER_LOGIN_PASSWORD https://599624599296.dkr.ecr.ap-south-1.amazonaws.com
         docker build -t 599624599296.dkr.ecr.ap-south-1.amazonaws.com/sample:PRUDHVI-PROJECT-${BUILD_NUMBER} .
         docker push 599624599296.dkr.ecr.ap-south-1.amazonaws.com/sample:PRUDHVI-PROJECT-${BUILD_NUMBER}
          
	  '''
     }   
   }
    stage('ecs deploy') {
      steps {
        sh '''
          chmod +x changebuildnumber.sh
          ./changebuildnumber.sh $BUILD_NUMBER
	  sh -x ecs-auto.sh
          '''
     }    
    }
}
}
