properties([pipelineTriggers([githubPush()])])

node('linux') 
{   
    stage('Test') 
	{    
	git 'https://github.com/rao03114/java-project.git'
	sh 'ant -f test.xml -v'
	junit 'reports/result.xml' 
	}   
		
		
    stage('Build') 
	{   
        sh 'ant -f build.xml -v'
    }   
		
		
    stage('Deploy') {
    sh 'aws s3 cp ${WORKSPACE}/dist/rectangle-$BUILD_NUMBER.jar s3://jenkins-s3bucket-llnp8fz4jq42'
    }
    
    
    stage('report')
	{
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '4eade9ba-f577-48a1-ae2e-690e108da3f0', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
        }
}
}
