node ('master')
 {
  
  def maven363 = tool name: "maven3.6.3"
  
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
  
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([githubPush()])])
    stage('Getting code from github')
    {
        git credentialsId: '961ce41b-f470-47ff-a5eb-0fcc38a08758', url: 'https://github.com/mk-devops-git/maven-web-application.git'
    }
    stage('maven Build'){
        sh "${maven363}/bin/mvn clean package"
    }
    stage('sonar report')
    {
        sh "${maven363}/bin/mvn sonar:sonar"
    }
    stage('UploadIntoArtifacoryRepository-Nexus'){
        sh "${maven363}/bin/mvn deploy"
    }
    stage('Deployment-TomCat')
    {
        sshagent(['959d8bce-28d1-4783-9f1f-6074599eb7e1'])
            {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.141.11:/opt/apache-tomcat-9.0.43/webapps"
            }
    }
  /*
 stage('EmailNotification')
 {
 mail bcc: 'devopstrainingblr@gmail.com', body: '''Build is over

 Thanks,
 Mithun Technologies,
 9980923226.''', cc: 'devopstrainingblr@gmail.com', from: '', replyTo: '', subject: 'Build is over!!', to: 'devopstrainingblr@gmail.com'
 }
 */
 
 }
