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
                // Aquí iría el comando npm install si instalamos el plugin de NodeJS
            }
        }
        stage('Aprobación Manual') {
            steps {
                input message: '¿Aprobar pase a producción?', ok: 'Desplegar'
            }
        }
        stage('Despliegue') {
            steps {
                echo 'Desplegando la aplicación en el entorno de producción...'
            }
        }
    }
}