pipeline {
  environment {
      namespace =  env.BRANCH_NAME.toLowerCase()
  }
     agent any
           
      stages {
        stage('CheckOut') {            
            steps { checkout scm }            
        }
    
        
        stage('Build') {
          when { anyOf { branch 'desenvolvimento'; branch 'hom'; branch "prd"; } }
          steps {
            script{                               
                    dir("node-project") {
                        dockerImage = docker.build "10.100.4.81:32000/portalapp:${env.namespace}"
                            dockerImage.push()                                                      
                    }
                }
          }
        }
            
       
        stage('Deploy') {
    when { anyOf { branch 'desenvolvimento'; branch 'hom'; branch "prd";; } } 
    steps {
        input "Efetuar o deploy para ${env.BRANCH_NAME}? (Requer Aprovação)"
        script {
            sh "microk8s kubectl rollout restart deployment/deploy-portalapp -n ${env.namespace}"
        }
    }
}
      }
}