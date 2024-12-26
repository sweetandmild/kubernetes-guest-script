pipeline {
    agent any

    tools {
        gradle 'gradle-7.6.1'
        jdk 'jdk-17'
    }

    environment {
        // 본인의 username으로 하실 분은 수정해주세요.
        DOCKERHUB_USERNAME = 'academyitwill'
        GITHUB_URL = 'https://github.com/2024-07-JAVA-DEVELOPER-155/kubernetes-guest-script.git'
        // deployment.yaml -> image: academyitwill/api-tester:v1.0.0        
    }

    stages {
        stage('Source Build') {
            steps {
                // 소스파일 체크아웃
                git branch: 'main', url: 'https://github.com/2024-07-JAVA-DEVELOPER-155/kubernetes-guest-source.git'

                // 소스 빌드
                // 755권한 필요 (윈도우에서 Git으로 소스 업로드시 권한은 644)
                sh "chmod +x ./gradlew"
                sh "gradle clean build"
            }
        }

        stage('Container Build') {
            steps {	
                // 릴리즈파일 체크아웃
                checkout scmGit(branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: "${GITHUB_URL}"]])

                // jar 파일 복사
                sh "cp ./build/libs/guest.jar ./build/docker/app.jar"

                // 컨테이너 빌드 및 업로드
                sh "docker build -t ${DOCKERHUB_USERNAME}/guest ./build/docker"
                script{
                    if (DOCKERHUB_USERNAME != "academyitwill") {
                        echo "docker push ${DOCKERHUB_USERNAME}/guest"
                    } else {
                        sh "docker push ${DOCKERHUB_USERNAME}/guest"
                    }
                }
            }
        }

        stage('K8S Deploy') {
            steps {
                // 쿠버네티스 배포 
                sh "kubectl apply -f ./deploy/k8s/namespace.yaml"
				sh "kubectl apply -f ./deploy/k8s/configmap.yaml"
				sh "kubectl apply -f ./deploy/k8s/secret.yaml"
				sh "kubectl apply -f ./deploy/k8s/service.yaml"
				sh "kubectl apply -f ./deploy/k8s/deployment.yaml"
            }
        }
    }
}