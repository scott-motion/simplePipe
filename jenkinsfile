pipeline {
    agent any

	stages {
	
    	stage('Checkout Test Automation from GIT') {
    		steps {
        		bat "git init" //this is ok if the git repo is already initialized
	            bat "git config http.sslVerify false"
                
    	     	git branch: 'development',
                	url: 'ssh://al004100@teamforge.corp.motion-ind.com:29418/test-automation'
	 		}
		}

        stage('Copy VariablePayRegressionTests_QAT.properties.properties and rename to config.properties') {
            steps {
                dir ("Motion.API.TestExecution/src/main/resources") {
                    fileOperations([fileDeleteOperation(includes: 'config.properties', excludes: '')])
                    fileOperations([fileRenameOperation(source: 'VariablePayRegressionTests_QAT.properties', destination: 'config.properties')])
                }
            }
        }    

		stage('Maven Compile Test Install Motion.API.TestExecution project execute Profile "VariablePayRegressionTests"') {
			steps {
				dir("Motion.API.TestExecution") {
				bat "mvn compile test -P VariablePayRegressionTests"
				}
    		}
		}
	
	}
	
// always, changed, fixed, regression, aborted, success, unsuccessful, unstable, failure, notBuilt, cleanup
	post {
//	    always {
//	        powershell 'Set-DisplayResolution -Width 1920 -Height 1080 -Force'
//	    }
        failure {
            slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} failed.  Runtime: ${currentBuild.durationString.minus(' and counting')}"
        }
        fixed {
            slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was the first success after failure. Runtime: ${currentBuild.durationString.minus(' and counting')}"
        }
        unstable {
            slackSend color: "warning", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was unstable. Runtime: ${currentBuild.durationString.minus(' and counting')}"
        }
    }

}