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

	    stage('sonar') {

	    	environment {
	    		scannerHome = tool 'sonar_scanner'
	    	}
	    	steps {
	    		withSonarQubeEnv('sonar_server') {
    				sh "${scannerHome}/bin/sonar-scanner"
				}
	    	}

	    }

	    stage('deploy') {
	    	steps {
	    		script {

	    			sshagent(['key1']) {
	    			def tomcatServerIp = '35.202.93.122' 
			        sh "ssh anurag@${tomcatServerIp} sudo rm -rf /opt/tomcat/webapps/*.war "
	    			sh "scp -o StrictHostKeyChecking=no target/helloWorld*.war anurag@${tomcatServerIp}:/opt/tomcat/webapps/helloWorld.war"
                    sh "ssh anurag@${tomcatServerIp} sudo systemctl restart tomcat.service"
					}
	    		}
	    	}
	    }
	}
}
