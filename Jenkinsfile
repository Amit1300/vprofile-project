pipeline {
    agent any

    tools{
        maven 'maven3'
        jdk 'jdk8'
    }

    environment {
        SNAP_REPO="vprofile-snapshot"
        NEXUS_USER="admin"
        NEXUS_PASS="nexus"
        RELEASE_REPO="vprofile-release"
        CENTRAL_REPO="vpro-maven-central"
       // NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "13.127.206.214"
        NEXUS_PORT="8081"
      //  NEXUS_REPOSITORY = "vprofile-release"
        NEXUS_GRP_REPO = "vprofile-group"
        NEXUS_CREDENTIAL_ID = "nexus"
        ARTVERSION = "${env.BUILD_ID}"

    }

    stages {
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

	stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
           stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'sonar'
          }

          steps {
            withSonarQubeEnv('sonar') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
            
            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }

            
          }
        }
         stage("nexus artifact upload"){
            steps{
                    nexusArtifactUploader(
        nexusVersion: 'nexus3',
        protocol: 'http',
        nexusUrl: "${NEXUS_URL}:${NEXUS_PORT}",
        groupId: 'com.visualpathit',
        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
        repository: "{RELEASE_REPO}",
        credentialsId: 'nexus',
        artifacts: [
            [artifactId: 'vprofile',
             classifier: '',
             file:'target/vprofile-v2.war',
             type: 'war']
        ]
     )
            }
         }

    }
}
