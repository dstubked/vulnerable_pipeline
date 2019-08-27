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
    
        stage('Aqua Scanner CLI') {
        /* scan image using scanner CLI */
            sh "echo Hello from the shell"
            sh "hostname"
            sh "echo $WORKSPACE"
            sh "docker run -v /var/run/docker.sock:/var/run/docker.sock registry.aquasec.com/scanner:4.2 scan -H http://aquasec-demo658-vm0.eastus.cloudapp.azure.com -U jenkins_scanner -P P@ssword --registry 'Docker Hub' dstubked/docker-test:latest --htmlfile results.html"
    }
    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '$WORKSPACE', reportFiles: 'results.html', reportName: 'Aqua HTML Report', reportTitles: ''])
}
