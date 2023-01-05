pipeline{
agent any

tools{
maven "maven3.8.6"
}
stages{

stage('1CloneCode'){
steps{
sh "echo 'Cloning of the Code'"
git "https://github.com/obinnaaliogor/maven-standalone-application.git"

}
}

stage('2Test&Build'){
steps{
sh "echo 'Running Junit testing'"
sh "echo 'Testing must pass to procced with Build'"
sh "mvn clean package"
}
}

stage('3CodeQualityReview'){
steps{
sh "echo 'Running Code Quality review and Analysis'"
sh "mvn sonar:sonar"

}
}
stage('4Upload2Nexus'){
steps{
sh "echo 'Uploading Artifacts to Artifactory'"
sh "mvn deploy"

}

}
/*
Iam not interesting in running deployment to tomcat since its a standalone application.
stage('Deploy2UAT'){
steps{
sh "echo ''Deployment to UAT environment"
deploy adapters: [tomcat9(credentialsId: 'Tomcat Credentials', path: '', url: 'http://172.31.34.244:8177')], contextPath: null, onFailure: false, war: 'target/*war'
}
}
stage('ApprovalGate'){
steps{
sh "echo 'Ready for review'"
timeout(time:5, unit: 'DAYS'){
input message: 'Application ready for deployment to production, Please kindly review and act accordingly'
}
}
}
stage('Deploy2Prod'){
steps{
sh "echo ''Deployment to PROD environment"
deploy adapters: [tomcat9(credentialsId: 'Tomcat Credentials', path: '', url: 'http://172.31.46.38:8177')], contextPath: null, onFailure: false, war: 'target/*war'
}
}
*/

}



post{

always{
emailext body: '''Hi all,
Please review the status of the build.
Thanks and Best Regards
Obinna''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), brokenTestsSuspects(), upstreamDevelopers(), previous()], subject: 'Build status', to: 'msa_team@gmail.com'

}
success{
emailext body: '''Hi all,
Great Job, build integration and Build was successfull
Thanks and Best Regards
Obinna''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), brokenTestsSuspects(), upstreamDevelopers(), previous()], subject: 'Build status', to: 'msa_team@gmail.com'
}
failure{
emailext body: '''Hi all,
The build failed, kindly review and resolve it accordingly
Thanks and Best Regards
Obinna''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), brokenTestsSuspects(), upstreamDevelopers(), previous()], subject: 'Build status', to: 'msa_team@gmail.com'

}

}

}
