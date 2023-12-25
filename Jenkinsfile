pipeline{
    agent{
        label "agent"
    }
    environment {
        APP_NAME = "webapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "hiastdevops"
        DOCKER_PASS = 'dockerhub'
        KUBE_CREDENTAILS = 'eyJhbGciOiJSUzI1NiIsImtpZCI6ImhmMk1TTmF3eE0wWmZ6NTR0MDJnNEx1X2VZdm1MN2VCZTBMTGM1c3dWc00ifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzAzNTM4MjU2LCJpYXQiOjE3MDM1MzQ2NTYsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0Iiwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImplbmtpbnMiLCJ1aWQiOiJhNzgzMmZlOC0yMTExLTQwMjUtYTg3NC1iMTFiYTc4NjExNzYifX0sIm5iZiI6MTcwMzUzNDY1Niwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6amVua2lucyJ9.i9jun_Fq2RPVYIXv_Psi5SMU9Vgt7fIem1VRB0CIV5PBnS-cgNVlsT7v5ebdm9ZL8z8O59KoY5g7fc6_ts_ihzB3BSeGfrBekd187TWptFsTvpfGrtZEmPhdzr0zJMUs8D-h95RTO6Ru7rXHAVw-pGrmWRdrVIs_Y8_rkiQHL7ZDX94d6b-45uJJY0e5lARcBWqUc-qZMAbhycq7NPOwYCh2bDm9b0ed0TCTg3ia2WYHpw8qcwzbAgoEXVu_6LiRFDWuIW-vsWN1Si8Zur0SucEKDk6MImqxhXUkoa6cumzNBYauX7shwRhU5Xl5Mfvm3yDt9xg0YG88zR--_ngebQ'
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