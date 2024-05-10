pipeline {
    agent any
     parameters {
        // Define boolean parameter.
        choice(name: 'RUN_BUILD', choices: ['True', 'False'], description: 'Select True to run the build, or False to skip.')
     }
    stages {    
        stage('Build') {
	    when {
                expression {params.RUN_BUILD == "true"}   
            }
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
	    when {
                expression {params.RUN_BUILD == "true"}   
            }
	    parallel{
                stage('Test_Failure') {
			steps {
				echo 'Testing 1..'
                		catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                    		sh "exit 1"
				}
                       }
		}
		stage('Test_Success') { 
			steps {
				echo 'Testing 2..'
				sh 'echo "This is m -"max"- artifact file" > arcfile.txt'
		        }
		}
            }
        }
        stage('Deploy') {
	    when {
                expression {params.RUN_BUILD == "true"}  
            }
            steps {
                echo 'Deploying....'
		archiveArtifacts artifacts: 'arcfile.txt', onlyIfSuccessful: true
            }
        }
	stage('Skipped'){
            when {
                expression {params.RUN_BUILD == "false"}
            }
            steps{
                echo "pipeline skipped..."
            }
        }
    }
	post {
        success{
            script{
                if (params.RUN_BUILD == "true"){
                    sendEmail()
                }
            }
        }
        failure{
            script{
                if (params.RUN_BUILD == "true"){
                    sendEmail()
                }
            }
        }
        unstable{
            script{
                if (params.RUN_BUILD == "true"){
                    sendEmail()
                }
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
