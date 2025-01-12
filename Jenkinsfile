pipeline {
    agent any

    tools {

        nodejs 'nodejs' // ' ' Le nom que vous avez donné à votre installation NodeJS dans Jenkins
        maven 'install maven' // Define Maven tool to be used
    }
   stages {
        stage("Clean up") {
            steps {
                deleteDir() // Clean up the workspace before starting
            }
        }

        stage("Checkout SCM") {
            steps {
                checkout scm
            }
        }
    stage('Build Frontend React') {
            steps {
                // Se déplacer dans le répertoire du frontend Angular
                dir("${env.WORKSPACE}/react-crud-web-api-master") {
                    // Installer les dépendances Angular
                    sh "npm install"

                    // Construire le projet Angular
                    sh "npm run build"
                }
            }
        }
      stage("Build Backend") {
            steps {
                // Navigate to the project directory and build the Maven project
                dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
                    sh "mvn clean install"
                }
            }
        }
      stage("Build Docker Image Backend") {
            steps {
                // Build the Docker image using the Dockerfile
                dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
                    sh "docker build -t backend ."
                }
            }
        }
        stage("Build Docker Image Frontend") {
            steps {
                // Build the Docker image using the Dockerfile
                dir("${env.WORKSPACE}/react-crud-web-api-master") {
                    sh "docker build -t frontend ."
                }
            }
        }

        stage("Run Docker Compose") {
            steps {
                // Run Docker Compose to start the application
                dir("nodejs-react-devops") {
                    sh "docker compose up -d"
                }
            }
        }
    }
}
