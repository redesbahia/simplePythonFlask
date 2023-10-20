pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                docker build -t simplepythonflask .
            }
        }
        
        stage('Executa Teste') {
            steps {
                docker run --rm -tdi -p5000:5000 --name test simplepythonflask
                sleep 10
                docker exec -ti test nosetests --with-xunit --with-coverage --cover-package=project test_users.py
                docker cp test:/courseCatalog/nosetests.xml .
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
            dpcker stop test
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'Ocorreu uma falha'
            docker stop test
        }
        changed {
            echo 'Things were different before...'
        }
    }
}

