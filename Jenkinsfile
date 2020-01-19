pipeline {
   agent any

   stages {
      stage('Git Checkout') {
         steps {
            echo 'Git checkout'
            git 'https://github.com/tambuzi/master-salle-ci'
         }
      }
      stage('Cleaning Package') {
         steps {
            echo "Cleaning Package"
            sh script: './mvnw clean package -DskipTests'
         }
      }
      stage ('Tests') {
          steps {
            sh label: '', script: './mvnw compile test'
            junit 'target/surefire-reports/*.xml'
          }
      }
      stage('Acceptance Test'){
         steps{
            echo 'Executing Acceptance Test'
            sh label:'', script: './mvnw -Dtest=ExampleResourceIT test'
            junit 'target/surefire-reports/*.xml'
         }
      }
      stage('Package') {
         steps {
            echo "Package"
            sh script: './mvnw package -DskipTests'
         }
      }
     stage('Build Docker Image') {
         steps {
            sh 'docker build -t tambuzi1997/maven:3.3-jdk-8 .'
         }
     }
     stage('Push Docker Image') {
         steps {
            withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerPass')]) {
               sh "docker login -u tambuzi1997 -p ${DockerPass}"
            }
            sh 'docker push tambuzi1997/maven:3.3-jdk-8'
         }
     }
     stage('Run Container on Server') {
        steps {
            sh 'docker run -p 8081:8081 -d -name maven tambuzi1997/maven:3.3-jdk-8'
        }
     }
   }
}