pipeline {
	agent any

	stages {

	    stage('git') {
	        steps {
	           script {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git_creds', url: 'https://github.com/anurag4418/hello_world.git']]])	              
	           }
	        }
	    }

	    stage('maven') {
	          steps {
	                script {
		                def mvnHome = tool name: 'maven3', type: 'maven'
		                sh "${mvnHome}/bin/mvn -version"
		                sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore=true clean package"	                       
	                }
	          }
	    }

	    stage('deploy') {
	    	steps {
	    		script {

	    			sshagent(['key1']) {
	    			def tomcatDevIp = '35.238.53.50'
	    			sh "scp -o StrictHostKeyChecking=no target/helloWorld*.war anurag@${tomcatDevIp}:/opt/tomcat/webapps/myweb.war"
	    			sh "ssh anurag@${tomcatDevIp} /opt/tomcat/bin/shutdown.sh"
	    			sh "ssh anurag@${tomcatDevIp} /opt/tomcat/bin/startup.sh"
					}
	    		}
	    	}
	    }
	}
}
