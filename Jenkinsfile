pipeline {

    agent any

    environment {
        MYSQL_ROOT_LOGIN = credentials('my-sql-account')
    }
    stages {

        stage('Deploy NodeJs app to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker container stop deploy_nodejs_app_to_dev_main-nginx-1 || echo "this container does not exist" '
                sh 'docker container stop deploy_nodejs_app_to_dev_main-nodejs_1 || echo "this container does not exist" '
                sh 'docker deploy_nodejs_app_to_dev_main-db_1 || echo "this container does not exist" '
                sh 'echo y | docker container prune '
                
                sh 'docker compose up'
                sh 'sleep 20'
                sh "docker exec -it nodejsmysql_db_1 mysql -u root -p123456a@ -e \"CREATE USER 'root'@'localhost' IDENTIFIED BY '123456a@';\""
                sh "docker exec -it nodejsmysql_db_1 mysql -u root -p123456a@ -e \"GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;\""
            }
        }
 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}

