pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.15.83.89', description: 'Staging Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ec2-user/anand.pem /var/lib/jenkins/workspace/pipeline-test/webapp/target/*.war ec2-user@${params.tomcat_dev}:/home/ec2-user/apache-tomcat-7.0.92/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo "Waiting for Stage implementation"
                    }
                }
            }
        }
    }
}
