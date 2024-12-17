pipeline {
    agent any

	triggers {
        githubPush() // Trigger the pipeline when a push event occurs
    }
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker')
        //FRONTEND_IMAGE = 'vaibhavnitor/frontend-three-tier-image'
        //BACKEND_IMAGE = 'vaibhavnitor/backend-three-tier-image'
        //DATABASE_IMAGE = 'vaibhavnitor/database-three-tier-image'
        //DATABASE = 'database_container'
        // GKE Cluster Details
        GKE_CLUSTER_NAME = 'my-first-cluster-1'
        GKE_ZONE = 'us-central1-c'
        GKE_PROJECT = 'poc-cluster-443705'
		

    }

    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'testing-2', url: 'https://github.com/C2-84177-Assignments/Three-tier-dotnet-SQL-16-12.git'
            }
		}

stage('Deploy to GKE') {
            steps {
                script {
                    // Authenticate with GKE
                    withCredentials([file(credentialsId: 'gke-credentials', variable: 'GKE_CREDENTIALS')]) {
                        // Authenticate with GCloud
                        sh '''
                            gcloud auth activate-service-account --key-file=${GKE_CREDENTIALS}
                            gcloud config set project ${GKE_PROJECT}
                            gcloud container clusters get-credentials ${GKE_CLUSTER_NAME} --zone ${GKE_ZONE}
                        '''
                        
                        // Deploy to GKE
                        sh '''
                            kubectl apply -f manifest/frontend.yaml
                            kubectl apply -f manifest/frontend-service.yaml
                            
                        '''
                        
                        // Verify deployments
                        sh '''
                            kubectl get deployments
                            kubectl get services
                            kubectl get pods
                        '''
						}
					}
				}
			}
  }
}
