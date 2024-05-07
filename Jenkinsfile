pipeline {
    agent any
     parameters {
        // Define boolean parameter.
        choice(name: 'RUN_BUILD', choices: ['True', 'False'], description: 'Select True to run the build, or False to skip.')
     }
    stages {
	 stage('Check RUN_BUILD') {
            steps {
                script {
                    if (params.RUN_BUILD == 'False') {
                        echo 'The build was skipped because RUN_BUILD is set to False.'
                        // Set the result to ABORTED to indicate the build was skipped
                        currentBuild.result = 'ABORTED'
                        // Use return here to exit from the pipeline execution
                        return
                    }
                }
            }
        }    
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
	success {
		when {
        	expression { params.RUN_BUILD == 'True' }
        	}
	}
	aborted {
                echo 'Stage aborted.'
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
