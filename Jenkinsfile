pipeline {
    agent any
    
    parameters{
            string(name: 'tomcat_test', defaultValue: 'yourEC2IP', description: 'Staging Server')
            string(name: 'tomcat_prod', defaultValue: 'yourEC2IP', description: 'Production Server')
        }

    triggers{
            pollSCM('* * * * *')
        }

    stages{
        stage('Build'){
            steps {
                sh "mvn clean package"
            }
            post {
                success{
                echo "Now Archiving..."
                archiveArtifacts artifacts: '**/target/*.war'
                }  
            }
        }
    
        stage('Deployments'){
            parallel{
                stage ('Deploy to staging'){
                   steps {
                     sh "scp -i /root/ssh/MyVPCKP.pem **/target/webapp.war ec2-user@${params.tomcat_test}:/var/lib/tomcat7/webapps/"
            }
        }
                stage ('Deploy to Prod'){
                  steps {
                    sh "scp -i /root/ssh/MyVPCKP.pem **/target/webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps/"
                }
    }

}
        }   
    }
}    
