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
            dir ('gateway-service') {
                def image = docker.build "gauthamdharsan/gateway-service:latest"
          
            }
        }
        
        stage('Push Image') {
            docker.withRegistry(credentialsId: 'docker', url: 'gauthamdharsan') {
            image.push()
            }
        }
        
        
        stage ('Run') {
            docker.image("gauthamdharsan/gateway-service:latest").run('-p 4444:4444 -h gateway --name gateway --link discovery --link accounts --link customer')
        }
     }

  

