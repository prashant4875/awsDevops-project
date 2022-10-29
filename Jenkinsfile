pipeline {
    agent any 
    tools {
        maven "MAVEN3"
        jdk  "OracleJDK8"
    }
    
    environment {
        SNAP_REPO = 'awsDevops-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'awsDevops-release'
        CENTRAL_REPO = 'awsDevops-proxy'
        NEXUSIP = '172.31.83.126'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'awsDevops-group'
        NEXUS_LOGIN = 'nexuslogin'

    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('Test') {
            steps {
                sh 'mvn -s setting.xml test'
            }
        }
        stage ('Checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
    }
}