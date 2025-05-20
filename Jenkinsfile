
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
                    python -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                    pip install --upgrade dvc gcsfs google-auth google-cloud-storage

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
                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                        dvc pull -v
                        '''
                    }
                }
            }
        }
    }
}