node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("dstubked/docker-test")
    }
    
    stage ('Aqua Scanner') {
        aqua customFlags: '', hideBase: false, hostedImage: '', localImage: 'dstubked/docker-test', locationType: 'local', notCompliesCmd: '', onDisallowed: 'fail', policies: '', register: false, registry: '', showNegligible: false
    }
    
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
