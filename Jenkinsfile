pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Instalación & Pruebas') {
            steps {
                echo 'Simulando instalación de dependencias y pruebas unitarias...'
            }
        }
        stage('Despliegue') {
            steps {
                echo 'Desplegando la aplicación en producción sin intervención manual...'
            }
        }
    }
}