pipeline {
    agent any
    environment {
    	DOCKER_REPOSITORY = "docker.io"
        DOCKER_REPOSITORY_URL = "docker.io/visionyuyu"
        REMOTE_CODE_REPOSITORY_URL = "https://github.com/yuyiwei305/piplinetest-poll-server.git"
        REMOTE_HELM_CHART_REPOSITORY_URL = "git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/rancher_catalog"
        REMOTE_HELM_CHART_REPOSITORY_GITHUB_URL="github.com/yuyiwei305/bookinfo-demo"
        GIT_TAG = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        PROJECT_NAME =  "web-demo"
        HELM_PZROJECT_NAME = "jenkins-web-demo"
        
  }
    stages {

        stage('Clone'){
            steps{
            echo "clone code"            
            }
       } 

  
        stage('Code Analysis') {    
            steps{
            echo "Code Analysis"
            }   
       }
  
        stage('Build Image') {
            steps{
            echo "3.Build Docker Image Stage"
            sh """
               docker build -t ${DOCKER_REPOSITORY_URL}/${PROJECT_NAME}:${GIT_TAG} .

               """
            }   
        }

        stage('Push Image') {
            steps{
            withCredentials([usernamePassword(credentialsId: '62b019d1-02fa-47a0-b5b9-019d5f546f4d', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {

            	echo "4.Push Image stage"

    			sh """
    			    docker login ${DOCKER_REPOSITORY} -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}


                            docker push ${DOCKER_REPOSITORY_URL}/${PROJECT_NAME}:${GIT_TAG}

    			   """
				}
            }
        }
  
        stage('Pudh helm chart') {
            steps{
            withCredentials([usernamePassword(credentialsId: 'ba4dfb37-b290-488b-946f-23b165c20f69', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
    			sh """
    			    git clone https://${GIT_USERNAME}:${GIT_PASSWORD}@${REMOTE_HELM_CHART_REPOSITORY_URL}

    			    cd rancher_catalog/${HELM_PZROJECT_NAME}

                    echo ${GIT_TAG}

                    sed -i "/.*options/a    - ${GIT_TAG}"  test.yaml

                    cat questions.yaml


    			   """
				}
            }
       }
    }
}


