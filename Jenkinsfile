pipeline{
    agent any

    environment{
        VENV_DIR = 'venv'
        TZ = 'UTC' 
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

        stage("Preparar Ambiente"){
            steps{
                script{
                    sh '''#!/bin/bash
                    # Limpa o ambiente existente
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

        stage("Validar Credenciais"){
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
    }
}