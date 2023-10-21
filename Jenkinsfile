pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t simplepythonflask .'
            }
        }
        
        stage('Executa Teste') {
            steps {
                sh 'docker run --rm -tdi -p5000:5000 --name test simplepythonflask'
                sh 'sleep 10'
                sh 'docker exec -i test nosetests --with-xunit --with-coverage --cover-package=project test_users.py'
                sh 'docker cp test:/courseCatalog/nosetests.xml .'
                junit nosetests.xml
            }
            
        }
    }
        post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'Finalizei com sucesso'
            sh 'docker stop test'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'Ocorreu uma falha'
            sh 'docker stop test'
        }
        changed {
            echo 'Things were different before...'
        }
    }
}

