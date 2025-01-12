pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Nom de l'installation NodeJS dans Jenkins
        
        
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

        // Étape de construction de l'image Docker pour le backend
        stage("Build Docker Image Backend") {
            steps {
                dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
                    sh "docker build -t backend ." // Construire l'image Docker pour le backend
                }
            }
        }

        // Étape de construction de l'image Docker pour le frontend
        stage("Build Docker Image Frontend") {
            steps {
                dir("${env.WORKSPACE}/react-crud-web-api-master") {
                    sh "docker build -t frontend ." // Construire l'image Docker pour le frontend
                }
            }
        }

        // Lancer Docker Compose pour déployer les services
        stage("Run Docker Compose") {
            steps {
                dir("nodejs-react-devops") {
                    sh "docker compose up -d" // Lancer Docker Compose en mode détaché
                }
            }
        }

        //Analyse SonarQube du backend (Node.js)
        stage("SonarQube Analysis Backend") {
    steps {
        script {
            // Utilisation du scanner SonarQube dans Jenkins
            sonarScanner (
                installationName: 'SonarQube Scanner', // Nom de l'installation SonarQube dans Jenkins
                projectKey: 'nodejs-backend', // Clé du projet dans SonarQube
                projectName: 'Node.js Backend', // Nom du projet dans SonarQube
                projectVersion: '1.0', // Version du projet
                sources: '.', // Répertoire des sources à analyser
                exclusions: '**/node_modules/**,**/test/**', // Exclusions pour l'analyse
                lcovReportPaths: 'coverage/lcov.info', // Chemin vers le rapport LCOV pour la couverture de test
                sonarHostURL: 'http://localhost:9000', // URL de votre serveur SonarQube
                login: 'sonarqube-token' // Token d'authentification pour SonarQube
            )
        }
    }
}

        
               

        // Analyse SonarQube du frontend (React)
        
    }
}
