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
    }
}
