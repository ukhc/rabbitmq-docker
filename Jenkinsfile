pipeline {
	agent any
    parameters {
    	string(name: 'appName', defaultValue: 'rabbitmq', description: 'the app name')
    }

    
	stages {
    	//stage "Build"
    
        	//sh "docker build -t ${imageName} ."
    
    	//stage "Push"

        	//sh "docker push ${imageName}"

    	stage("Deploy") {
    		steps {
    			// make a temp file (overwrite if necessary)
    			sh "cp -f kubernetes-deployment.yaml kubernetes-deployment.yaml.tmp"
    			// change the ingress url to match the environment
    			script {
    				if (env.BUILD_ENVIRONMENT == 'staging') {
    					sed -i '' "s/${appName}.ukhc.org/staging-rabbitmq.ukhc.org/" kubernetes-deployment.yaml.tmp
    				}
    				if (env.BUILD_ENVIRONMENT == 'local') {
    					sed -i '' "s/${appName}.ukhc.org/local-rabbitmq.ukhc.org/" kubernetes-deployment.yaml.tmp
    					sed -i '' "s/replicas:.*/replicas: 2/" kubernetes-deployment.yaml.tmp
    				}
    			}
    			// apply
    			sh "kubectl apply -f kubernetes-deployment.yaml.tmp"
    			// remove the temp file
    			sh "rm kubernetes-deployment.yaml.tmp"
    		}
    	}
    }
}