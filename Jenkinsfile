pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Unit Tests') {
            agent {
                label 'master'
            }
            steps {
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }
        stage('build') {
            agent {
                label 'master'
            }
            steps {
               sh 'ant -f build.xml -v'
            }
	    post {
        	always {
            		archiveArtifacts artifacts: 'dist/*.jar' , fingerprint: true
        	}
	    }
        }
        stage('deploy') {
            agent {
                label 'master'
            }
            steps {
                sh "echo Branch: ${env.BRANCH_NAME}"
                sh "pwd"
                sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
            }
        }
	state('Running on CentOS') {
	  agent{
	    label 'CentOS'
	  }
	  steps {
	    sh "wget http://alvinwang6.mylabserver.com/rectangles/all/rectangles_${env.BUILD_NUMBER}.jar"
	    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
	  }
	}
    }

}
