pipeline{
    agent any
    
    stages{
        stage('clone code'){
            steps{
                echo'clone the code'
                checkout scm
            }
        }
        stage('buld the code'){
            steps{
                sh'''
                pip install flask
                python3 -m py_compile app.py
                '''
            }
        }

        stage('Scan tha code'){
            steps{
                withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube') {
                    sh 'sonar-scanner'
}

            }
        }

        stage('docker buld image'){
            steps{
                sh'''
                docker build -t netflix .
                docker tag netflix purnaspr/netflix:1.0
                '''
            }
        }

        stage('docker image'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'Docker', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh'''
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker push purnaspr/netflix:1.0
                    '''
}
            }
        }


    }
}