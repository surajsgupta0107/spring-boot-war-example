pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        stage("Test"){
            steps{
                sh "mvn test"
                // slackSend channel: 'youtubejenkins', message: 'Job Started'
            }
        }
        stage("Build"){
            steps{
                sh "mvn package"
            }
        }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'tomcat9details', path: '', url: 'http://10.0.2.15:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
        stage("Deploy on Production"){
             input {
                message "Should we continue?"
                ok "Yes we Should"
            }
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'tomcat9details', path: '', url: 'http://10.0.2.15:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'youtubejenkins', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
            // slackSend channel: 'youtubejenkins', message: 'Job Failed'
        }
    }
}
