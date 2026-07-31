// pipeline {
//   agent any
    
//   tools {nodejs "node"}
    
//   stages {
        
//     stage('Git') {
//       steps {
//         git branch: 'main', credentialsId: 'ssh-git', url: 'https://github.com/tolaoguntunde/node-cls'
//       }
//     }
     
//     stage('Build') {
//       steps {
//         sh 'npm install'
//          //sh '<<Build Command>>'
//       }
//     }  
    
//   }
// }
pipeline {
  agent any
  tools { nodejs "node" }
  stages {
    stage('Git') {
      steps {
        sh 'git clone https://github.com/tolaoguntunde/node-cls.git .'
        sh 'pwd'
      }
    }
    stage('build') {
        steps {
            sh 'docker build -t tolaoguntunde/node-app .'
            sh 'docker push docker.io/tolaoguntunde/node-app:latest'
            sh 'docker image rm tolaoguntunde/node-app'
            
        }
    }
    stage('run') {
        steps {
            sh'docker run -d --name nodeapp -p 3001:3000 tolaoguntunde/node-app'   
        }
    }
  }
  
}
