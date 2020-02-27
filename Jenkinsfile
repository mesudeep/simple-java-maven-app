// node('docker'){
//     stage('checkout'){
//         checkout scm
//     }
//     stage('Build'){
//         docker.image('lokeshkamalay/uta-test:good').inside(){
//             sh 'mvn clean install'
//         }
//     }
// }


pipeline {
    agent {
        docker {
            image 'lokeshkamalay/uta-test:good'
            label 'docker'
        }
    }
    environment {
        DOCKER_CREDS = credentials('dockerhub-id')
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Create-Image') {
            agent {
                label 'docker'
            }
            steps {
                sh """
                    docker build -t lokeshkamalay/uta-test:dockertest .
                    docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PWD}
                    docker push lokeshkamalay/uta-test:dockertest
                """
            }
        }
    }
    post {
        success {
            echo "Good Job Docker!"
        }
        failure {
            echo "I did mistake, let me fix"
        }
        always {
            deleteDir()
        }
    }
}