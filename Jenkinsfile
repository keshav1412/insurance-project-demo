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
    stage('stage to deploy'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    stage('checkout regression test source code'){
        git 'https://github.com/keshav1412/my-selenium-test-app.git'
    }
    
    stage('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    
    stage('execute selenium test script'){
        sh 'java -jar target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }

    stage('checkout'){
        git 'https://github.com/keshav1412/insurance-project-demo.git'
    }
    
     stage('Deploy to Prod'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }  
}
