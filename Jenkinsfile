pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_new"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }
        }
           stage('Test') {
            steps {
                sh "ls ; mvn test"
            }
            post{
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco (execPattern: 'target/jacoco.exec')
                  }
             }
        }
         stage('Docker Build and Push') {
            steps {
                // withDockerRegistry handles login automatically using your credentialsId
                withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
               // withDockerRegistry(credentialsId: 'docker-hub', url: 'https://docker.io/v2/') {
                    
                    // 1. Build and tag with Git Commit
                   // docker build --no-cache -t username/app:latest .
                   sh "docker build -t dockersmpv/numeric-app:${GIT_COMMIT} ."
                  //  sh "docker build --no-cache -t dockersmpv/numeric-app:${GIT_COMMIT} ."
                    
                    // 2. Add the 'latest' tag to the image we just built
                    sh "docker tag dockersmpv/numeric-app:${GIT_COMMIT} dockersmpv/numeric-app:latest"
                    // sh "docker tag dockersmpv/numeric-app:${GIT_COMMIT} username/numeric-app:latest"
                   // sh "docker tag dockersmpv/numeric-app:${GIT_COMMIT}"
                    
                    // 3. Push both tags to Docker Hub
                  //  sh "docker push dockersmpv/numeric-app:${GIT_COMMIT}"
                    sh "docker push dockersmpv/numeric-app:latest"
                    
                    sh "echo 'Successfully pushed tags: ${GIT_COMMIT} and latest'"
                }
            }
        }
        // test origin  
        //stage('Docker Build and Push') {
        //    steps {
         //     withDockerRegistry(credentialsId: 'docker-hub', url: 'https://docker.io/')  {
         //       sh 'printenv'
         //       sh 'docker build -t dockersmpv/numeric-app:""$GIT_COMMIT"" .'
          //      sh 'docker push dockersmpv/numeric-app:""$GIT_COMMIT""'
           // }
        // }
     // } //test
        
   }   
}
