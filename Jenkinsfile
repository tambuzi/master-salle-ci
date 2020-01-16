pipeline {
   agent any
    
       stages {
          stage('Compile & Test') {
             steps {
                    echo 'Hello From Jenkinsfile'
                    git 'https://github.com/tambuzi/master-salle-ci'
                    sh label: '', script: './mvnw compile test'
                    junit 'target/surefire-reports/*.xml'
             }
          }
          stage('Package') {
              steps {
                echo "Building $version"
                sh script: './mvnw package -DskipTests'
              }
          }
     
   }
}
