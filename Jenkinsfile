pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        
        stage('invoke analyzer') {
        steps {
         script {
              withCredentials([usernamePassword(credentialsId: 'kiuwan',
                        passwordVariable: 'PASSWORD',
                        usernameVariable: 'USERNAME')]) {
                         def returnCode = sh(returnStatus: true, script:'${AGENT_HOME}/bin/agent.sh -s \"${WORKSPACE}\" -n \"${JOB_NAME}\" -l \"${BUILD_TAG}\" --user \"$USERNAME\" --pass \"$PASSWORD\"')
                         //echo "Building PR #${env.getEnvironment()}"
                         
                         switch(returnCode){
                                case 0:  
                                    currentBuild.result = 'SUCCESS'
                                    break
                                case 14:
                                    currentBuild.result = 'UNSTABLE'
                                    break
                                case 1:
                                case 10:
                                    currentBuild.result = 'FAILURE'
                                    break
                                case 11:
                                case 12:
                                    currentBuild.result = 'NOT_BUILT'
                                    break
                                case 13:
                                    currentBuild.result = 'ABORTED'
                                    break
                                default:
                                    currentBuild.result = 'NOT_BUILT'
                                }//end switch
                           echo currentBuild.result
                        }//end withCredentials
                  }//end script
             }//end step
        }//end stage
       
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
