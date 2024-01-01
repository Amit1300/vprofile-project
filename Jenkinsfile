pipeline {
    agent any

    tools{
        maven 'maven'
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
        NEXUS_URL = "172.31.46.175"
        NEXUS_PORT="8081"
      //  NEXUS_REPOSITORY = "vprofile-release"
        NEXUS_GRP_REPO = "vprofile-group"
        NEXUS_CREDENTIAL_ID = "nexus"
        ARTVERSION = "${env.BUILD_ID}"
    }

    stages {
        stage('BUILD') {
            steps {
               
                
                        sh 'mvn clean install -DskipTests'
                               
            }
        }
    }
}
