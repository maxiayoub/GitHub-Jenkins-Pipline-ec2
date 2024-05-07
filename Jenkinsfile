pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                    sh "exit 1"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }

        post {
        always {
            script {
                // Call the email function for each method
                sendEmail("Jenkins Pipeline Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}")
            }
        }
        
    }
}
 

def sendEmail() {
     mail to: "maximousfr.ayoubmehanne@gmail.com"
     subject: ${subject}
     def body = """
                    Hello
                    This is  a Jenkins Pipline Notification for ${env.JOB_NAME} status
                    
                    Pipeline executed on: ${new Date().format(' HH:mm:ss   dd-MM-YYYY ')}
                    Pipeline Status: ${currentBuild.result ?: 'Unknown'}
                    Job Name: ${env.JOB_NAME}
                    Build Number: build ${env.BUILD_NUMBER}
                    More info at ${env.BUILD_URL}

		    By Maximous ElKess Ayoub
                """                   
}
