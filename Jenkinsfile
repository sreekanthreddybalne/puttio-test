pipeline {
    agent none
    stages {

        stage('Build and Publish') {
            when {
                expression { true }  //set to false to disable this stage
            }
            agent {
                label 'winbuilda02'
            }
            environment {
                PYTHON = "D:/jenkins/workspace/ccs/Datalakes/python64_repo"
            }
            steps {
                bat "${env.PYTHON}//python --version"
                bat "${env.PYTHON}//python -m venv datalake_env"
                bat "call .\\datalake_env\\Scripts\\activate.bat"
                bat "${env.PYTHON}//python -m pip install --extra-index-url=https://artifactory.factset.com/artifactory/api/pypi/python-fds/simple -r requirements.txt"
                bat "${env.PYTHON}//python setup.py sdist bdist_wheel"
                bat "${env.PYTHON}//python -m pip install twine"
                dir('dist'){
                    bat "dir"
                    bat "${env.PYTHON}//python -m twine upload --repository python-fds *.whl"
                    bat "${env.PYTHON}//python -m twine upload --repository python-fds *.tar.gz"
                }
                
            }
            post {
                cleanup {
                    /* clean up our workspace */
                    deleteDir()
                }
            }        
        }              
    }    
}