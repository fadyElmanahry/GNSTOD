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
				script {
				script: '''
					dotnet build -c Release /p:Version=${BUILD_NUMBER}
					dotnet publish -c Release --no-build

				  '''
					}
					}
	  
		}

		stage('UnitTest') {
		steps {
			script {
			  bat label: 'Unit Test using Dotnet CLI',
			script: '''
			
			'''
				}
			}
		}
	 
		stage('compress') {
		steps {
				zip zipFile: 'TestJenkins.zip', archive: false, dir: 'C:\\Users\\Access\\source\\repos\\TestJenkins\\TestJenkins\\bin\\Debug\\net5.0\\'
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
			  "C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe" -verb:sync -source:package="C:\\Users\\Access\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\mobilydeploy\\TestJenkins.zip" -dest:contentPath='TestJenkins'
					 '''
				}
		}

    }   
}