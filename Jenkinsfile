node{
    
    stage('checkout'){
        git 'https://www.github.com/keshav1412/insurance-project-demo.git'
    }
    stage('maven build'){
        sh 'mvn clean package'
    }
    stage('containerize'){
        sh 'docker build -t rathorekeshav14/insure-me:1.0 .'
    }
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhub')]) {
        sh "docker login -u rathorekeshav14 -p ${dockerhub}"
        sh 'docker push rathorekeshav14/insure-me:1.0'
        }
    }
    stage('stage to test'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
     stage('Deploy to Prod'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }  
}
