node {
    def mavenHome=tool name: "MAVEN-3.9.16"
	stage('git checkout') {
		git branch: 'master', url: 'https://github.com/DNadendla/maven-webapplication-project-kkfunda.git'
	}

	stage('clean & package') {
		sh 'echo "Maven Clean & Compile Stage"'
		sh "${mavenHome}/bin/mvn clean package"
	}

	stage('sonarqube report') {
		sh "${mavenHome}/bin/mvn sonar:sonar"
	}

	stage('nexus-artifactory backup') {
		sh "${mavenHome}/bin/mvn deploy" 
	}

	stage('tomcat deployment') {
		sh """
		curl -u kk:password \
		--upload-file /var/lib/jenkins/workspace/scipted-way-PL-2/target/maven-web-application.war \
		"http://43.205.177.15:8080/manager/text/deploy?path=/maven-web-application&update=true"

		""" 
	}
}
