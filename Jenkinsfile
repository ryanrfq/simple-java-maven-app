pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    	stage('Test') {
    	        steps {
    	    	    sh 'mvn test'
    	        }
    	        post {
    	    	    always {
    	    		    junit 'target/surefire-reports/*.xml'
    	    	    }
    	        }
    	}
        stage('Manual Approval'){
            steps {
                input(message:"Lanjutkan ke tahap Deploy?")
            }
        }
    	stage('Deploy') {
    	        steps {
    	            sh './jenkins/scripts/deliver.sh'
                    sleep(time:60,unit:'SECONDS')
    	        }
    	}
    }
}
