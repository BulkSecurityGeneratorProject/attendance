node {
    // uncomment these 2 lines and edit the name 'node-4.4.7' according to what you choose in configuration
    // TEST 
    def nodeHome = tool name: 'node-4.4.5', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    env.PATH = "${nodeHome}/bin:${env.PATH}"

    stage 'check tools'
    sh "whoami"
    sh "node -v"
    sh "npm -v"


    stage 'checkout'
    checkout scm

    stage 'clean'
    sh "./mvnw clean"

    stage 'npm install'
    sh "npm install"
    sh "npm update"
    sh "npm install -g grunt-cli"
    sh "npm install -g gulp-cli"
    
    stage 'backend tests'
    sh "./mvnw test"

    stage 'frontend tests'
    sh "gulp test"

    stage 'package'
    sh "./mvnw -Dmaven.tests.skip=true package"


   // stage 'sonar analysis'
    sh "sudo ./mvnw sonar:sonar -Dsonar.host.url=http://10.150.4.31:9000"

    stage 'deploy'
    sh "scp -i ~/.ssh/Develop.pem target/*.war ubuntu@10.150.1.169:attendance.war"
    sh "ssh -i ~/.ssh/Develop.pem ubuntu@10.150.1.169 'java -jar attendance.war'"
   
}
