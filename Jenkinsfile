pipeline{
    agent{
        label "agent"
    }
    environment {
        APP_NAME = "webapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "hiastdevops"
        DOCKER_PASS = 'dockerhub'
        KUBE_CREDENTAILS = 'k8sconfigpwd'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Git SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/MohamadAlturky/webApp'
            }

        }

        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
        // stage('Deploy to k8s'){
        //     steps{
        //         script{
        //             kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
        //         }
        //     }
        // }
        stage('Deploy to K8s') {
            steps{
                script {
                    sh "kubectl --kubeconfig=${KUBE_CREDENTAILS} apply -f deploymentservice.yaml"
                }
            }
    }

    }
}