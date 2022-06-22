pipeline {
    agent { label liabuildslave || liabuildagent }
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Build') {
            steps {
                // Clean before build
                cleanWs()
                checkout scm
                echo "Building ${env.JOB_NAME}..."
            }
        }
        stage('Java8 build') {
            buildName '${version_acronym}-${version_branch}-${version_major}.${version_minor}-r${GIT_TIMESTAMP}-${GIT_SHORT_COMMIT}-${BUILD_NUMBER}'
            parallel {
                stage('Build Aurora') {
                  options {
                  timeout(time: 1, unit: 'HOURS')   // timeout on this stage
                }
                steps {
                  echo "Building Aurora"
                }  
                stage('Branch B') {
                    agent {
                        label "for-branch-b"
                    }
                    steps {
                        echo "On Branch B"
                    }
                }
            }
         }      
    }  
    post {
      always {
        junit '**/reports/junit/*.xml'
      }
    } 
}
