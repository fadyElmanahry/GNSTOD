pipeline {

    agent {
        node {
            label 'master'
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        
	   stage('Nuget Restore') {
	  steps {
			bat label: 'Nuget Restore', 
			script: '''
			  dotnet restore "TestJenkins.sln"
			  echo "Nuget Done Starting Msbuild *************"
			''' 
}
		}

		stage('Build') {
			steps {
			bat label: 'build', 
				script: '''
					dotnet build -c Release /p:Version=1.0.0.0
					dotnet publish -c Release --no-build

				  '''

					}
	  
		}

		stage('UnitTest') {
		steps {

			  bat label: 'Unit Test using Dotnet CLI',
			script: '''
			
			'''

			}
		}
	 
		stage('compress') {
		steps {
			bat '''del /f C:\\Users\\Access\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\ModilyMultiBranch_main\\TestJenkins.zip'''
				zip zipFile: 'TestJenkins.zip', archive: false, dir: 'C:\\Users\\Access\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\ModilyMultiBranch_main\\TestJenkins\\bin\\Release\\net5.0\\'
			}

		}

	  
		stage('Deploy') 
		{
			when {
					branch 'main'
				}
		   steps {
			bat label: 'MsDeploy',
			script: ''' 
			  "C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe" -verb:sync -source:package="C:\\Users\\Access\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\ModilyMultiBranch_main\\TestJenkins.zip" -dest:contentPath='TestJenkins'
					 '''
				}
		}

    }   
}
