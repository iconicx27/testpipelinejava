pipeline {
    agent any
    tools {
        maven "M3"
    }

    stages {
        //-----------------------------------------stage-1:Checkout---------------------------------------------
        stage('Checkout') {
            steps {
                checkout scm    
            }
        }
        
        //------------------------------------------stage-2:Build------------------------------------------------
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        //-------------------------------------------stage-3:Install in Dev---------------------------------------
        stage('Install in Dev') {
            when {
                branch 'dev'
            }
            steps{
                sh "mvn install"
            }
        }

        //----------------------------------------------stage-4:Deploy to Tomcat------------------------------------
        stage('Deploy to tomcat') {
            when{
                branch 'main'
            }
            steps {
                sh "mvn install"
                echo "deploy stage"
                deploy adapters: [tomcat9 (
                    credentialsId: 'tomcatID',
                    path: '',
                    url: 'http://51.20.4.171:8088/'
                )],
                contextPath: 'testdeploy',
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
