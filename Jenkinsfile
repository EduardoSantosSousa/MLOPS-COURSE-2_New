pipeline{
    agent any

    environment{
        VENV_DIR = 'venv'
        TZ = 'UTC'  // Define o timezone globalmente
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
                    sh '''
                    python -m venv ${VENV_DIR} --clear  // Limpa o ambiente existente
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip setuptools wheel
                    pip install --force-reinstall "gcsfs==2024.2.0"  // Versão FIXA
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
                        python -c "
import os
from datetime import datetime, timezone
from google.cloud import storage

print('Credenciais:', os.environ['GOOGLE_APPLICATION_CREDENTIALS'])
client = storage.Client()
buckets = client.list_buckets(max_results=1)
print('Sucesso! Primeiro bucket:', next(buckets).name if buckets else 'Nenhum')
                        "
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
