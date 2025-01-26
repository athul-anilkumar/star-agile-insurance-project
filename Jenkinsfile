node{
    
    def mavenHome
    def mavenCMD
    def docker
    def dockerCMD
    def tagName
    
    stage('prepare enviroment'){
        echo 'initialize all the variables'
        mavenCMD = "/usr/share/maven/bin/mvn"
        docker = tool name: 'docker' , type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        dockerCMD = "${docker}/bin/docker"
        tagName="3.0"
    }
    
    stage('git code checkout'){
            echo 'checkout the code from git repository'
            git 'https://github.com/athul-anilkumar/star-agile-insurance-project.git'
    }
    
    stage('Build the Application'){
        echo "Cleaning... Compiling...Testing... Packaging..."
        //sh 'mvn clean package'
        sh "${mavenCMD} clean package"        
    }
    
    stage('Containerize the application'){
        echo 'Creating Docker image'
        sh "${dockerCMD} build -t athulanil/insure-me:${tagName} ."
    }
    
    stage('Pushing it ot the DockerHub'){
        echo 'Pushing the docker image to DockerHub'
        withCredentials([string(credentialsId: 'dock-password', variable: 'dockerHubPassword')]) {
        sh "${dockerCMD} login -u athulanil -p ${dockerHubPassword}"
        sh "${dockerCMD} push athulanil/insure-me:${tagName}"
            
        }
        
    stage('Configure and Deploy to the test-server'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml'
    }
        
        
    }
}




