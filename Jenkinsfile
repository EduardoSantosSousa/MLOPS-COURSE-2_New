
pipeline{
    agent any

    environment{
        VENV_DIR = 'venv'
    }

    stages{
        stage("Cloning from Github......."){
            steps{
                script{
                    echo 'Cloning from Github.......'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/EduardoSantosSousa/MLOPS-COURSE-2_New.git']])
                }
            }
        }
        stage("Making a virtual environment......."){
            steps{
                script{
                    echo 'Making a virtual environment.......'
                    sh '''
                    . ${VENV_DIR}/bin/activate
                    export TZ=UTC

                    # --- Verificação das Credenciais ---
                    echo "Verificando existência do arquivo..."
                    ls -l ${GOOGLE_APPLICATION_CREDENTIALS} || echo 'Arquivo não encontrado!'

                    echo "Conteúdo parcial do arquivo (primeiras 2 linhas):"
                    head -n 2 ${GOOGLE_APPLICATION_CREDENTIALS} || echo 'Falha ao ler arquivo'

                    echo "Testando autenticação com o Google Cloud..."
                    python -c "

                    import os
                    from google.cloud import storage
                    client = storage.Client()
                    buckets = list(client.list_buckets(max_results=1))
                    print('Conexão bem-sucedida! Buckets encontrados:', len(buckets))
                    " || echo 'Falha na autenticação'
                    '''                    
                }
            }
        }


        stage("DVC Pull"){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'DVC Pul.....'
                        sh'''
                        . ${VENV_DIR}/bin/activate
                        export TZ=UTC
                        dvc pull -v
                        '''
                    }
                }
            }
        }
    }
}