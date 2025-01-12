pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Nom de l'installation NodeJS dans Jenkins
         sonarQubeScanner 'sonarqube'
    }
    environment {
        SONAR_SCANNER_OPTS = "-Xmx1024m" // Options de mémoire pour le scanner
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
                withSonarQubeEnv("sonarqube") { // Charger la configuration SonarQube
                    dir("${env.WORKSPACE}/nodejs-express-sequelize-mysql-master") {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=nodejs-backend \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${env.SONAR_AUTH_TOKEN} \
                            -Dsonar.exclusions=node_modules/**,test/** \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                        """
                    }
                }
            }
        }

        // Analyse SonarQube du frontend (React)
        stage("SonarQube Analysis Frontend") {
            steps {
                withSonarQubeEnv("sonarqube") { // Charger la configuration SonarQube
                    dir("${env.WORKSPACE}/react-crud-web-api-master") {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=react-frontend \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${env.SONAR_HOST_URL} \
                            -Dsonar.login=${env.SONAR_AUTH_TOKEN} \
                            -Dsonar.exclusions=node_modules/**,build/** \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                        """
                    }
                }
            }
        }
    }
}
