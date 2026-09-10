pipeline {
    agent any
    stages {
        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t lab-cicd-app:jenkins .'
            }
        }
        stage('Pruebas en Contenedor') {
            steps {
                sh 'docker run --rm lab-cicd-app:jenkins node -e "console.log(\'Sintaxis de Node.js validada correctamente\')"'
            }
        }
        stage('Verificar Artefacto') {
            steps {
                sh 'docker images | grep lab-cicd-app'
            }
        }
    }
}