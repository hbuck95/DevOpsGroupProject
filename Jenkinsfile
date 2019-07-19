pipeline{
	agent any
	environment {
		ORG = "hazardd"
		NEW_PRIZE_VER = "small"
		NEW_NUM_VER = "big"
		NEW_TEXT_VER = "lower"
	}
	stages{	
		stage('Build Client'){
                        steps{
				sh 'sudo docker build ./client/. -t $ORG/client:v1'
                        }
                }
                stage('Build Server'){
                        steps{
				sh 'sudo docker build ./server/. -t $ORG/server:v1'				
                        }
                }
		stage('Build Textgens'){
			steps{
				sh 'sudo docker build ./textgen-lower/. -t $ORG/textgen:lower'
				sh 'sudo docker build ./textgen-upper/. -t $ORG/textgen:upper'
			}
		}
		stage('Build Numgens'){
                        steps{
                                sh 'sudo docker build ./numgen_small/. -t $ORG/numgen:small'
                                sh 'sudo docker build ./numgen_big/. -t $ORG/numgen:big'
                        }
                }
		stage('Build Prizegens'){
                        steps{
                                sh 'sudo docker build ./prizegen-small/. -t $ORG/prizegen:small'
                                sh 'sudo docker build ./prizegen-big/. -t $ORG/prizegen:big'
                        }
                }
		stage('Build Notification Server'){
			steps{
				sh 'sudo docker build ./notification_server/. -t $ORG/notification_server:v2'
			}
		}
		stage('Build DB Connector'){
                        steps{
                                sh 'sudo docker build ./db_connector/. -t $ORG/db-connector:v1'
                        }
                }
                stage('Push'){
                        steps{
                                sh 'sudo docker push $ORG/client:v1'
				sh 'sudo docker push $ORG/server:v1'
				sh 'sudo docker push $ORG/textgen:lower'
				sh 'sudo docker push $ORG/textgen:upper'
				sh 'sudo docker push $ORG/numgen:small'
				sh 'sudo docker push $ORG/numgen:big'
				sh 'sudo docker push $ORG/prizegen:small'
				sh 'sudo docker push $ORG/prizegen:big'
				sh 'sudo docker push $ORG/notification_server:v2'
				sh 'sudo docker push $ORG/db-connector:v1'
                        }
                }
		stage('Update Prize Gen'){
                        steps{
                                sh "kubectl set image deployments/prizegen prizegen=$ORG/prizegen:$NEW_PRIZE_VER"
                        }
                }
                stage('Update Textgen'){
                        steps{
				sh "kubectl set image deployments/textgen textgen=$ORG/textgen:$NEW_TEXT_VER"
                        }
                }
                stage('Update Numgen'){
                        steps{
                                sh "kubectl set image deployments/numgen numgen=$ORG/numgen:$NEW_NUM_VER"
                        }
                }
	}
}
