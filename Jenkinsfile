pipeline {
    agent any // Esto significa que el pipeline se ejecutará en cualquier agente disponible

    stages {
        stage('Preparar SSH') {
            steps {
                // Asegúrate de que el plugin SSH Agent esté instalado y la credencial configurada
                sshagent(['webserver']) { // Reemplaza con el ID de tu credencial
                    script {
                        // Opcional: Probar la conexión SSH a uno de los servidores
                        sh 'ssh -o StrictHostKeyChecking=no vagrant@192.168.56.20 "echo Conectado"'
                    }
                }
            }
        }

        stage('Actualizar Servidor 1') {
            steps {
                sshagent(['webserver']) {
                    sh 'ssh -o StrictHostKeyChecking=no admin_servidor@tu_servidor_1 "sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y"'
                }
            }
        }

        stage('Actualizar Servidor 2') {
            steps {
                sshagent(['webserver']) {
                    sh 'ssh -o StrictHostKeyChecking=no vagrant@192.168.56.21 "sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y"'
                }
            }
        }

        // Agrega más etapas para cada servidor que quieras actualizar
        // Puedes parametrizar esto para hacerlo más dinámico, pero para un inicio, esto es sencillo
    }

    post {
        always {
            echo 'Proceso de actualización de paquetes finalizado.'
        }
        success {
            echo 'Actualización de paquetes completada exitosamente.'
        }
        failure {
            echo '¡ATENCIÓN! Fallo en la actualización de paquetes.'
            // Aquí podrías agregar notificaciones por email, Slack, etc.
        }
    }
}
