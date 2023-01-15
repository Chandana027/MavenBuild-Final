node('') {
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
		sh 'mvn sonar:sonar -Dsonar.host.url=http://40.122.144.156:9000/ -Dsonar.login=a306aa2e3722ab3b15950b84354dade255256574'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage('Docker Build'){
        sh ' docker --version '
        sh ' docker build -t mavenbuild . '
        }
	
       stage('Create Container '){
        sh ' docker run -d -p 9000:8080  --name dockercontainerchandana mavenbuild '
        }
	
	stage ('Deployment'){
		ansiblePlaybook colorized: true, disableHostKeyChecking: true, playbook: 'deploy.yml'
	}
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "build-alerts@example.com"
		    )
	}
}
