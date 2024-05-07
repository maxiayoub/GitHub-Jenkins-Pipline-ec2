pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
		sh 'echo "artifact file" > arcfile.txt'
            }
        }
        stage('Test') {
            steps {
		parallel(
                Test1: { 
			echo 'Testing 1..'
                	catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                    	sh "exit 1"
                       }
		       },
		Test2: { 
			echo 'Testing 2..'
		       }
		)
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
	post {
        always {
            script {
		archiveArtifacts artifacts: 'arcfile.txt', onlyIfSuccessful: true
                // Call the email function for each method
                sendEmail()
            }
        }
	}
}

def sendEmail() {
     emailext to: "maximousfr.ayoubmehanne@gmail.com" , subject: "Jenkins Pipeline Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}", attachmentsPattern: 'arcfile.txt',
     body: """
                    Hello
                    This is  a Jenkins Pipeline Notification for ${env.JOB_NAME} status
                    
                    Pipeline executed on: ${new Date().format(' HH:mm:ss   dd-MM-YYYY ')}
                    Pipeline Status: ${currentBuild.result ?: 'Unknown'}
                    Job Name: ${env.JOB_NAME}
                    Build Number: build ${env.BUILD_NUMBER}
                    More info at ${env.BUILD_URL}
		    By Maximous ElKess Ayoub
                """                   
}
