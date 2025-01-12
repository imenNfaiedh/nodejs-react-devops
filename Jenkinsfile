pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Nom de l'installation NodeJS dans Jenkins
        maven 'install maven' // Nom de l'installation Maven dans Jenkins
    }

    stages {
        stage("Clean up") {
            steps {
                deleteDir() // Nettoyer l'espace de travail avant de commencer
            }
        }

        stage("Checkout SCM") {
            steps {
                checkout scm // Vérifier le code source du gestionnaire de contrôle de version
            }
        }

        // stage('Build Frontend React') {
        //     steps {
        //         dir("${env.WORKSPACE}/react-crud-web-api-master") {
        //             sh "npm install" // Installer les dépendances Node.js
        //             sh "npm run build" // Construire le projet React
        //         }
        //     }
        // }

        // stage("Build Backend") {
        //     steps {
        //         dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
        //             sh "npm install" // Installer les dépendances Node.js
        //             sh "npm run build" // Construire le backend (si applicable)
        //         }
        //     }
        // }

        stage("Build Docker Image Backend") {
            steps {
                dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
                    sh "docker build -t backend ." // Construire l'image Docker pour le backend
                }
            }
        }

        stage("Build Docker Image Frontend") {
            steps {
                dir("${env.WORKSPACE}/react-crud-web-api-master") {
                    sh "docker build -t frontend ." // Construire l'image Docker pour le frontend
                }
            }
        }

        stage("Run Docker Compose") {
            steps {
                dir("nodejs-react-devops") {
                    sh "docker compose up -d" // Lancer Docker Compose en mode détaché
                }
            }
        }

         stage("SonarQube Analysis") {
            steps {
                // Perform SonarQube analysis
                withSonarQubeEnv("sonarqube") {
                    dir("${env.WORKSPACE}") {
                        sh sh "sonar-scanner"
                    }
                }
            }
        }
    }
}
