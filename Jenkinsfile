pipeline {
   agent any
	options {
             skipDefaultCheckout true
	}
       stages {
      stage('Git Checkout') {
         steps {
           git 'https://github.com/arunimaU/maven-multi-module-example.git'
		}
	}
	         stage('Please Provide Approval for build'){

          steps{

            script{

                def userInput

  try {

    userInput = input(

        id: 'Proceed1', message: 'Build Approval', parameters: [

        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please Confirm you agree with this']

        ])

} catch(err) {

    def user = err.getCauses()[0].getUser()

    userInput = false

    echo "Aborted by: [${user}]"

}

          }

        }

        }                 


	stage ('Build')
	   
	    {steps{
                sh '/opt/maven/bin/mvn clean package -Dmaven.test.skip=true'
	    }    }
      stage('Please Provide Approval for Release in SIT'){

          steps{

            script{

                def userInput

  try {

    userInput = input(

        id: 'Proceed1', message: 'SIT Approval', parameters: [

        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please Confirm you agree with this']

        ])

} catch(err) {

    def user = err.getCauses()[0].getUser()

    userInput = false

    echo "Aborted by: [${user}]"

}

          }

        }

        }                 



     stage ('Release') 
     
     {steps{
	        sh '/opt/maven/bin/mvn --batch-mode release:clean release:prepare release:perform'
	     
          }
          }
       stage('Deployment-SIT'){

            steps{

sshPublisher(publishers: [sshPublisherDesc(configName: 'server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sh transfer.sh', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }

          }
       }}
