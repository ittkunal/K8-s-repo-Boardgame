pipeline {
    agent any

    environment {
        SONAR_HOME = tool "Sonar"
        ECR_REPO = '211125735014.dkr.ecr.ap-southeast-1.amazonaws.com/boardgame'
        AWS_REGION = 'ap-southeast-1'
        IMAGE_TAG = 'latest'
        IMAGE_NAME = "${ECR_REPO}:${IMAGE_TAG}"
    }

    stages {

        stage("checkout") {
            steps {
                git url: 'https://github.com/ittkunal/K8-s-repo-Boardgame.git', branch: 'main'
            }
        }

        stage("Static Code Analysis") {
            steps {
                withSonarQubeEnv('Sonar') {
                    sh """
                        ${SONAR_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=boardgame \
                        -Dsonar.projectName=boardgame \
                        -Dsonar.sources=.
                    """
                }
            }
        }

        stage("Sonar Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage("Maven Build") {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage("ECR login") {
            steps {
                sh """
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS --password-stdin ${ECR_REPO}
                """
            }
        }

        stage("Docker Build") {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage("Trivy Scan") {
            steps {
                sh "trivy image --exit-code 1 --severity HIGH,CRITICAL ${IMAGE_NAME}"
            }
        }

        stage("Push to ECR") {
            steps {
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                withCredentials([file(credentialsId: 'kube-creds', variable: 'KUBECONFIG')]) {
                    sh """
                        export KUBECONFIG=$KUBECONFIG
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                    """
                }
            }
        }
    }
}
