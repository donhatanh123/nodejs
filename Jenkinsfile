pipeline {

    agent any

    environment {
        MYSQL_ROOT_LOGIN = credentials('my-sql-account')
    }
    stages {

        stage('Packaging/Pushing imagae') {

            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t donhatanh2000/nodejs .'
                    sh 'docker push donhatanh2000/nodejs'
                }
            }
        }

        stage('Deploy MySQL to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull mysql'
                sh 'docker network create dev || echo "this network exists"'
                sh 'docker container stop nhatanh-mysql || echo "this container does not exist" '
                sh 'echo y | docker container prune '
                sh 'docker volume rm nhatanh-mysql-data || echo "no volume"'

                sh "docker run --name nhatanh-mysql --network dev -v nhatanh-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_LOGIN_PSW} -e MYSQL_DATABASE=db_example  -d --restart always mysql "
                sh 'sleep 20'
                sh "docker exec -i nhatanh-mysql mysql -u root -p123456a@ -e \"CREATE USER 'root'@'localhost' IDENTIFIED BY '123456a@';\""
                sh "docker exec -i nhatanh-mysql mysql -u root -p123456a@ -e \"GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;\""
            }
        }

        stage('Deploy nodejs to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull donhatanh2000/nodejs'
                sh 'docker container stop nhatanh-nodejs || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --name nhatanh-nodejs -p 3000:3000 --network dev --restart always donhatanh2000/nodejs'
            }
        }
        
         stage('Deploy nginx to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull nginx'
                sh 'docker container stop nhatanh-nginx || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --name nhatanh-nginx -p 80:80 --network dev --restart always nginx'
                sh 'docker cp ./nginx.conf nhatanh-nginx:/etc/nginx/nginx.conf'
                sh "docker restart nhatanh-nginx"
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
