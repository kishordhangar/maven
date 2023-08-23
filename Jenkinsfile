node(){

	def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	
	stage('Code Checkout'){
		checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHubCreds', url: 'https://github.com/kishordhangar/maven']])
	}
	stage('Build Automation'){
		sh """
			ls -lart
			mvn clean install
			ls -lart target

		"""
	}
	
	stage('Code Scan'){
		withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
			sh "${sonarHome}/bin/sonar-scanner"
		}
		
	}
	
	stage('Code Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://192.168.1.14:8080/')], contextPath: 'Planview', onFailure: false, war: 'target/*.war'
	}
}
