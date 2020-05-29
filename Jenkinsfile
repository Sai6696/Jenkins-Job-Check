def jobName = JOB_NAME
def projectName = jobName.split('/')[0]
def mvn_version = 'Maven' 
def application = 'Jenkins-Job-Check'
def muleversion = '4.2.2'
def businessGroup = 'Speridian'
def workerType = 'Micro'
def workers = '1'
node{
  stage('Checkout'){
                 echo  "Build: ${projectName} for branch ${BRANCH_NAME}"
                 git(	
                 url: "https://github.com/Sai6696/Jenkins-Job-Check.git",
				 credentialsId: 'Github',               
				 branch: "${BRANCH_NAME}"				 
				)
			bat 'git clean -f'		            
			bat 'git reset --hard'              
			bat 'git checkout .'             
    }
    stage('Build'){
                 echo "echo Building ${BRANCH_NAME}..."
                 bat "mvn clean install -DskipTests=true"
    }
    stage('Test'){
                 echo "echo Testing ${BRANCH_NAME}..."
                 bat "mvn clean test"
      }
     stage('Deploy'){
                 if("${BRANCH_NAME}" == 'develop'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        withCredentials([usernamePassword(credentialsId:'Anypoint', usernameVariable: 'anypoint_usr', passwordVariable: 'anypoint_psw')]){
                        bat "mvn clean deploy -Denvironment=DEV -Dusername=${anypoint_usr} -Dpassword=${anypoint_psw} -Dapplication=${application}-dev -Dmuleversion=${muleversion} -DbusinessGroup=${businessGroup} -DworkerType=${workerType} -Dworkers=${workers} -DmuleDeploy "
                        }            
                    }
                }
                else if("${BRANCH_NAME}" == 'qa'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        bat "mvn clean deploy -Denvironment=QA -Dapplication=${application}-qa -Dmuleversion=${muleversion} -DbusinessGroup=${businessGroup} -DworkerType=${workerType} -Dworkers=${workers} -DmuleDeploy"
                    }
                }
                else if("${BRANCH_NAME}" == 'sit'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        bat "mvn clean deploy -Denvironment=SIT -Dapplication=${application}-sit -Dmuleversion=${muleversion} -DbusinessGroup=${businessGroup} -DworkerType=${workerType} -Dworkers=${workers} -DmuleDeploy"
                    }
                }
                else if("${BRANCH_NAME}" == 'master'){
                    echo "echo Deploying to ${BRANCH_NAME}..."
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                        echo "${tool mvn_version}"
                        bat "mvn clean deploy -Denvironment=PROD -Dapplication=${application} -Dmuleversion=${muleversion} -DbusinessGroup=${businessGroup} -DworkerType=${workerType} -Dworkers=${workers} -DmuleDeploy"
                    }
                }             
            
    }
}