node {
    def app

    stage('Clone repository') {
        /* Clone repository to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("dstubked/docker-test")
    }
    
    /*stage ('Aqua Scan') {
        aqua locationType: 'hosted', registry: 'Docker Hub', hostedImage: 'dstubked/docker-test',  notCompliesCmd: '', onDisallowed: 'fail', hideBase: false, showNegligible: false
    }*/
    stage('Aqua Scanner CLI') {
            sh "echo Hello from the shell"
            sh "hostname"
            sh "echo $WORKSPACE"
            sh "echo $JENKINS_HOME"
            sh "docker run -e BUILD_JOB_NAME=ExampleVulnerablePipeline -e BUILD_URL=$BUILD_URL -e BUILD_NUMBER=$BUILD_NUMBER --rm -v /var/run/docker.sock:/var/run/docker.sock registry.aquasec.com/scanner:4.2 scan --host http://aquasec-demo658-vm0.eastus.cloudapp.azure.com --local dstubked/docker-test:latest --no-verify --html --register --user jenkins_scanner --password P@ssword > "${env.BUILD_NUMBER}".html"
            archiveArtifacts ""${env.BUILD_NUMBER}".html"
    }
    
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
        /*stage('Aqua Scanner CLI') {
            sh "echo Hello from the shell"
            sh "hostname"
            sh "echo $WORKSPACE"
            sh "echo $JENKINS_HOME"
            sh "docker run -e BUILD_JOB_NAME=ExampleVulnerablePipeline -e BUILD_URL=$BUILD_URL -e BUILD_NUMBER=$BUILD_NUMBER --rm -v /var/run/docker.sock:/var/run/docker.sock registry.aquasec.com/scanner:4.2 scan --host http://aquasec-demo658-vm0.eastus.cloudapp.azure.com --registry 'Docker Hub' dstubked/docker-test:latest --no-verify --register --html --user jenkins_scanner --password P@ssword > regresults.html"
    }
    archiveArtifacts 'regresults.html'*/
    /*publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '$WORKSPACE', reportFiles: 'results.html', reportName: 'Aqua HTML Report', reportTitles: ''])*/
}
