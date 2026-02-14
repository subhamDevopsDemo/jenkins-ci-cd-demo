pipeline {
    agent any
    tools{
        maven "maven"
    }

    stages {
        //SCM Checkout
        stage('GIT Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/subhamDevopsDemo/jenkins-ci-cd-demo.git']])
            }
        }

        //Maven Build
        stage("Maven Build"){
            steps{
                script{
                    bat 'mvn clean install'
                }
            }
        }
        //Deploy To Tomcat
        stage("Deploy To Tomcat"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-credencials', path: '', url: 'http://localhost:9090/')], contextPath: 'jenkinsCiCd', war: '**/*.war'
            }
        }
    }

    post{
        always{
            emailext attachLog: true, body: '''<html>
    <body>
        <p>Build Status: ${BUILD_STATUS}</p>
        <p>Build Number: ${BUILD_NUMBER}</p>
        <p>Check the <a href="${BUILD_URL}">console output</a> </p>
    </body>
</html>''', mimeType: 'text/html', replyTo: 'subhammohanty2511@gmail.com', subject: 'Pipeline Status: {BUILD_STATUS}', to: 'subhammohanty2511@gmail.com'
        }
    }
}
