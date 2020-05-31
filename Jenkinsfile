node {
      stage('Checkout') {
            git url: 'https://github.com/leo-coutinho/Sample-Spring-Microservices.git', credentialsId: 'gauthamdharsan', branch: 'master'
        }

        stage('Build') {
            sh 'mvn clean install'

            def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }

        stage('Build') {
            dir ('gateway-service') {
                def app = docker.build "gauthamdharsan/gateway-service:${env.version}"
            }
        }
        
        stage('Docker Push') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
            sh 'docker push gauthamdharsan/gateway-service:${env.version}'
        
        stage ('Run') {
             docker.image("gauthamdharsan/gateway-service:${env.version}").run('-p 4444:4444 -h gateway --name gateway --link discovery --link accounts --link customer')
        }
     

    }


