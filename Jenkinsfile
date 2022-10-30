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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'

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
                sh 'mvn -s settings.xml test'
            }
        }
        stage ('Checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile-repo \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                 }
            }
        }
    }
}