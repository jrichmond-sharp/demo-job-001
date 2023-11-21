pipline{
    agent any
    environment{
        staging_server="147.182.223.168"
        
    }
    
    stages{
        stage('Deploy to Remote'){
            steps{
                sh 'scp ${WORKSPACE}/* jenkins@${staging_server}:/var/www/vhosts/jenkins-web01.sharpinnovations.com/httpdocs'
            }
        }
    }
}