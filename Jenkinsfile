pipeline {
    agent any

    tools {
        maven "M3"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
            git branch:'main',
                url:'https://github.com/iconicx27/testpipelinejava.git'
            }            
        }

        stage('Build') {
            steps{
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        stage('Deploy') {
            steps {
                deploy adapters: [tomcat9 (
                    credentialsId: 'tomcatCredentials',
                    path: '',
                    url: 'http://172.174.150.7:8088/'
                )],
                contextPath: 'tomcatdeploybytejas',
                onFailure: 'false',
                war: '**/*.war'
            }
            
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
    }
}
