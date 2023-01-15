node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		sh 'mvn sonar:sonar -Dsonar.host.url=http://40.122.144.156:9000 -Dsonar.login=a306aa2e3722ab3b15950b84354dade255256574'
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
