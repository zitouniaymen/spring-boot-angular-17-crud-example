pipeline {
    agent any

    environment {
        // Définir les variables d'environnement JAVA_HOME, MAVEN_HOME, et Docker Hub credentials
        JAVA_HOME = 'C:\\Java\\jdk-17.0.9_windows-x64_bin\\jdk-17.0.9'
        MAVEN_HOME = 'C:\\apache-maven-3.8.8\\apache-maven-3.8.8'
        PATH = "${env.PATH};${JAVA_HOME}\\bin;${MAVEN_HOME}\\bin"
        DOCKER_CREDENTIALS_ID = 'FranceNanterre@2023'
        DOCKER_IMAGE = 'zitouni/bsn'
        DOCKER_TAG = "${env.GIT_BRANCH ?: 'latest'}"
    }

    stages {
stage('Checkout') {
    steps {
        // Cloner le code source depuis le dépôt, en utilisant la branche develop
        checkout([$class: 'GitSCM', branches: [[name: 'develop']], userRemoteConfigs: [[url: 'https://github.com/zitouniaymen/spring-boot-angular-17-crud-example.git']]])
    }
}


        stage('Build') {
            steps {
                // Se déplacer dans le répertoire du projet avant de lancer Maven
                dir('spring-boot-server') {
                    bat "${MAVEN_HOME}\\bin\\mvn clean install"
                }
            }
        }

    stage('Test') {
            steps {
                // Se déplacer dans le répertoire du projet avant de lancer Maven
                dir('spring-boot-server') {
                    bat "${MAVEN_HOME}\\bin\\mvn test"
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Déploiement de l\'application...'
                // Ajouter des étapes de déploiement si nécessaire
            }
        }
    }

    post {
    
        failure {
            echo 'Le pipeline a échoué.'
        }
        success {
            echo 'Le pipeline a réussi.'
        }
    }
}