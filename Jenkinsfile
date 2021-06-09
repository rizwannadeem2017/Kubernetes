pipeline {
    agent any
    environment {
        
        DOCKER_IMAGE_NAME = "rizwan1989/riz-tomcat"
        registry = "registry.hub.docker.com/rizwan1989/riz-tomcat"
        
       
    }

    stages {

//         stage('repo clone from github') {
           
//             //chechoutscm
//            steps {
//               //  git([url: 'git@github.com:rizwannadeem2017/Kubernetes.git', branch: 'main', credentialsId: 'github'])
//                 //git([url: 'https://github.com/rizwannadeem2017/Kubernetes.git', branch: 'master', credentialsId: 'github'])
//            }  
//         }

        stage('Build Docker Image') {
           
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    
                }
            }
        }
        stage('Push Docker Image') {
            
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                          app.push("${env.BUILD_NUMBER}")
                        //app.push("apiv1")
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes Environment') {
            steps {
                 sh "env"
                 sh "sed -i 's/build_version/${env.BUILD_NUMBER}/g' tomcat.yaml"
                 sh "kubectl apply -f tomcat.yaml"
                 
//                 kubernetesDeploy(
//                     kubeconfigId: 'kubernetes-1',
//                     configs: 'tomcat.yaml',
//                     enableConfigSubstitution: true
//                   )
            }
          }
        stage('Remove Unused docker image from docker agent') {
           
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
//                 sh "docker rmi $registry1:latest"
            }
        }
    }
 }
