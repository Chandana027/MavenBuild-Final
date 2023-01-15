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
		sh 'mvn sonar:sonar -Dsonar.host.url=http://20.29.72.44:9000 -Dsonar.login=d785e2e435133fac3642b3aed4e5323188cfdd80'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
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
