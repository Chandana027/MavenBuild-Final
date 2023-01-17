def containerName="mavenbuild"
def tag="latest"
def dockerHubUser="chandanas45"
def httpPort="8090"node('') 
node{
	stage ('checkout code'){
		git credentialsId: '949aec28-f21c-4e1d-9c1a-b78e5f306221', url: 'https://github.com/Chandana027/MavenBuild-Final.git'
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
// 		sh 'mvn sonar:sonar -Dsonar.host.url=http://40.122.144.156:9000/ -Dsonar.login=a306aa2e3722ab3b15950b84354dade255256574'
	}

	stage ('Archive Artifacts'){
// 		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage("Image Prune"){
         sh "docker image prune -a -f"
         }
	
	stage('Image Build'){
        sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
        echo "Image build complete"
        }
	
        stage('Push to Docker Registry'){
        	withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
            		sh "docker login -u $dockerUser -p $dockerPassword"
            		sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            		sh "docker push $dockerUser/$containerName:$tag"
            		echo "Image push complete"
        }
                }
        node('KubernetesVM'){
        	stage('Run App'){
            		sh """
         	 kubectl create deployment kubernetes-bootcamp --image=docker.io/updhanu/$containerName:$tag --port=8090
               		kubectl get pods
            	    """
                }
        }
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed chandanaa !!!",
		      to: "chandanas.bongale@gmail.com"
		    )
	}
	
// 	stage ('Deployment'){
// 		ansiblePlaybook colorized: true, disableHostKeyChecking: true, playbook: 'deploy.yml'
// 	}
}
