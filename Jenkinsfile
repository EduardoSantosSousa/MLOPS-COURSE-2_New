pipeline{
    agent any

    environment{
        VENV_DIR = 'venv'
        TZ = 'UTC'
        GCP_PROJECT = 'serious-cat-455501-d2'
        GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
        KUBECTL_AUTH_PLUGIN = "/usr/lib/google-cloud-sdk/bin"


    }

    stages{
        stage("Cloning from Github......."){
            steps{
                script{
                    checkout scmGit(
                        branches: [[name: '*/main']], 
                        userRemoteConfigs: [
                            [credentialsId: 'github-token', 
                            url: 'https://github.com/EduardoSantosSousa/MLOPS-COURSE-2_New.git']
                        ]
                    )
                }
            }
        }

        stage("Prepare Environment"){
            steps{
                script{
                    sh '''#!/bin/bash
                    # Cleans the existing environment
                    python -m venv ${VENV_DIR} --clear  
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip setuptools wheel
                    pip install --force-reinstall "gcsfs==2024.2.0"  
                    pip install --upgrade dvc google-auth google-cloud-storage
                    pip install -e .
                    '''
                }
            }
        }

        stage("Validate Credentials"){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        sh '''
                        . ${VENV_DIR}/bin/activate
                        python -c "import os; print('Credenciais:', os.environ['GOOGLE_APPLICATION_CREDENTIALS'])"
                        '''
                    }
                }
            }
        }

        stage("DVC Pull"){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        sh '''
                        . ${VENV_DIR}/bin/activate
                        dvc pull -v
                        '''
                    }
                }
            }
        }

        stage("Build and Push Image to GCR"){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Build and Push Image to GCR'
                        sh '''
                        export PATH=$PATH:${GCLOUD_PATH}
                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                        gcloud config set project ${GCP_PROJECT}
                        gcloud auth configure-docker --quiet
                        docker build -t gcr.io/${GCP_PROJECT}/ml-project:latest .
                        docker push gcr.io/${GCP_PROJECT}/ml-project:latest .
                        '''
                    }
                }
            }
        }

        stage("Deploy to Kubernetes....."){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Deploy to Kubernetes'
                        sh '''
                        export PATH=$PATH:${GCLOUD_PATH}:${KUBECTL_AUTH_PLUGIN}
                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                        gcloud config set project ${GCP_PROJECT}
                        gcloud container clusters get-credentials ml-app-cluster --region us-central1
                        kubectl apply -f deployment.yaml
                        '''
                    }
                }
            }
        }

    }
}