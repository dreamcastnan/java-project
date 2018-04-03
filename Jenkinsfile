pipeline {
    agent {
        label 'master'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage ('Echo something') {
            steps {
                sh "echo Branch: ${env.BRANCH_NAME}"
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }
        stage('build') {
            agent {
                label 'master'
            }
            when {
                branch 'development'
            }
            steps {
               sh 'ant -f build.xml -v'
            }
 
        }
        stage('deploy') {
            when {

            }
            steps {
                sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'dist/*.jar' , fingerprint: true 
        }
    }
}
