node {
    
    def registryProjet='cyprien962/'
    def IMAGE="${registryProjet}hackathon-ipssi-equipe6:version-${env.BUILD_ID}"
    
    stage('Clone') {
        checkout scm
    }
    
    def img = stage('Build') {
        docker.build("$IMAGE", '.')
    }
    
    stage('Run') {
        img.withRun("--name run-$BUILD_ID -p 8000:80") { c ->
            
        }
    }
    
    stage('Push') {
        docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_ID') {
            img.push 'latest'
            img.push()
        }
    }
}