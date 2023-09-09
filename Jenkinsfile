pipeline {

    agent any

    environment {
        MYSQL_ROOT_LOGIN = credentials('my-sql-account')
    }
    stages {

        stage('Deploy NodeJs app to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker container stop nodejsmysql_nginx_1 || echo "this container does not exist" '
                sh 'docker container stop nodejsmysql_nodejs_1 || echo "this container does not exist" '
                sh 'docker nodejsmysql_db_1 || echo "this container does not exist" '
                sh 'echo y | docker container prune '
                
                sh 'docker docker compose up'
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

