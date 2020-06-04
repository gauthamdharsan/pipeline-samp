node {
        stage('Checkout') {
            git url: 'https://github.com/gauthamdharsan/pipeline-samp.git', credentialsId: 'git', branch: 'master'
        }

        stage('build') {
            sh 'mvn clean package'

            def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
            
        }
        
        stage('Image') {
            dir ('.') {
                def app = docker.build "gauthamdharsan/gateway-service:${env.BUILD_NUMBER}"
          
            }
        }
        
        stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push()
            
            }
        }
        
        stage ('Run') {
            docker.image("gauthamdharsan/gateway-service:${env.BUILD_NUMBER}").run('-p 4444:4444 -h gateway --name gateway --link discovery --link accounts --link customer')
        }
     }

  

