node{
    stage('checkout'){
        git 'https://github.com/pranitasm/Project.git'
    }
    stage('maven build'){
        sh 'mvn clean package'
    }
    stage ('containerize'){
      //   sh 'docker build -t pranitamudgalkar/insure-me:1.0 .'
    }
    stage('Release'){
     withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPwd')]) {
       // sh "docker login -u pranitamudgalkar -p ${dockerHubPwd}"
       // sh 'docker push pranitamudgalkar/insure-me:1.0'
           }
        }
        
    stage ('Deploy to Test'){
        ansiblePlaybook become: true, credentialsId: 'ubuntu-ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
        
        
    }
    stage ('checkout code for Regression Testing'){
        git 'https://github.com/pranitasm/my-seleniumtest.git'
    }
    stage ('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    stage ('execute selelium test script'){
     sh 'java -jar target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'   
    }
    
    stage('Prod checkout'){
        git 'https://github.com/pranitasm/Project.git'
    }
    stage ('Deploy to Prod'){
        ansiblePlaybook become: true, credentialsId: 'ubuntu-ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
        
        
    }
    
}
