node {

    withMaven(maven:'maven') {

        stage('Checkout') {
            git url: 'https://github.com/gauthamdharsan/pipeline-samp.git', credentialsId: 'gauthamdharsan', branch: 'master'
        }

        stage('Build') {
            sh 'mvn clean install'

            def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }

        stage('Image') {
            dir ('gateway-service') {
                def app = docker.build "localhost:5000/gateway-service:${env.version}"
                app.push()
            }
        }

        stage ('Run') {
             docker.image("localhost:5000/gateway-service:${env.version}").run('-p 4444:4444 -h gateway --name gateway --link discovery --link accounts --link customer')
        }
     

    }

}
