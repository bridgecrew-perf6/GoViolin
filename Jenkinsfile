pipeline {
    agent any
    parameters {
        booleanParam(name: 'build_image', defaultValue: false, description: 'whether to build the image or not')
        booleanParam(name: 'push_image', defaultValue: false, description: 'whether to push the image to docker hub or not')
    }
    environment{
        DOCKERHUB_REGISTRY='mostafaw'
        IMAGE_NAME = 'goviolin'
        DOCKERHUB_CREDENTIALS='dockerhub_cred'
        EMAIL = 'mostafa.w.k000@gmail.com'
    }
    stages {
        stage('Build the Docker image') {
            when {
                expression {
                    params.build_image
                }
            }
            steps {
                script
                {
                    sh 'docker build -t ${DOCKERHUB_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} .'
                }
            }
           post {
                success {
                    echo "======== Build Success ========"
                    // mail to: "${EMAIL}",subject: "GoViolin Pipeline FaiSuccededled",body: "The pipeline managed to build the image"
                }
                failure {
                    echo "======== Build Failed ========"
                    // mail to: "${EMAIL}",subject: "GoViolin Pipeline Failed",body: "The pipeline failed to build the image"
                }
           }
        }
        stage('Push The image to Docker Hub') { 
            when {
                expression {
                    params.build_image
                    params.push_image
                }
            }
            steps { 
                
                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                { 
                    sh """
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker push ${DOCKERHUB_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}
                    """    
                } 
            }
             post {
                success {
                    echo "======== Push Success ========"
                    // mail to: "${EMAIL}",subject: "GoViolin Pipeline Succeded",body: "The pipeline managed to push the image to Docker Hub"
                }
                failure {
                    echo "======== Push Failed ========"
                    // mail to: "${EMAIL}",subject: "GoViolin Pipeline Failed",body: "The pipeline failed to push the image to Docker Hub"
                }
           }
        } 
	}
}