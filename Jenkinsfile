pipeline {
        agent any

    tools {
                maven 'M3'
        }

        parameters {
                string(name: "JIRA_ISSUE_KEY", description: "String parameter")
                choice(name: "ENV_NAME", choices: ["B01", "B02", "D01", "D02"], description: "Multi-choice parameter")
                string(name: "RELEASE_PACKAGE", description: "String parameter")
        }

    stages {
        stage('Deployment Beginning') {
            steps {
                echo "Deployment started"
            }
        }
        stage('Initialize') {
                steps{
                        sh '''
                        echo "PATH=${M2_HOME}/bin:${PATH}"
                        echo "M2_HOME= ${M2_HOME}"
                        '''
                }
        }
        stage('Preparation') {
            steps {
		        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'git@github.com:prsomash/SampleMavenProject.git']]])
                
		    }
        }
        stage('Build') {
            steps {
                // Run the maven build
                sh 'mvn --version'
                sh "'mvn' -Dmaven.test.failure.ignore test install"
            }
        }
    }
    post {
        success {
            script {
                if (! params.JIRA_ISSUE_KEY.isEmpty()) {
                    println " Deployment is successful "
                }
            }
        }
        failure {
            script {
                if (! params.JIRA_ISSUE_KEY.isEmpty()) {
                        println " Deployment failed "
                }
            }
        }
    }
}
