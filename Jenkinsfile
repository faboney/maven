pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                         git 'https://github.com/krishnain/maven.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins unable to download dev code from github', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'git.admin@gmail.com'
                        exit(1)
                    }
                }
               
            }
        }
        stage('ContinuousBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'mvn package'
                    }
                    catch (Exception e2)
                    {
                        mail bcc: '', body: 'Jenkins unable to create an artefact from code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'deve.team@gmail.com'
                        exit(1)
                    }
                }
                
               
            }
        }
        stage('ContinuousDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                    deploy adapters: [tomcat9(credentialsId: 'b0aeb251-e61c-4ca9-bbc9-5df530f29992', path: '', url: 'http://172.31.19.118:8080')], contextPath: 'testapp', war: '**/*.war'
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'Jenkins unable to deploy into tomcat on QAServer', cc: '', from: '', replyTo: '', subject: 'Deployment failed', to: 'middleware.team@gmail.com'
                        exit(1)
                    }
                }
                
            }
        }
        stage('ContinuousTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/declarativepipeline/testing.jar'
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Selenium scripts are failing ', cc: '', from: '', replyTo: '', subject: 'Testing failed', to: 'qa.team@gmail.com'
                        exit(1)
                    }
                }
                
            }
        }
        stage("ContinuousDelivery'")
        {
            steps
            {
                script
                {
                    try
                    {
                       deploy adapters: [tomcat9(credentialsId: 'b0aeb251-e61c-4ca9-bbc9-5df530f29992', path: '', url: 'http://172.31.16.232:8080')], contextPath: 'prodapp', war: '**/*.war'
                    }
                    catch(Exception e5)
                    {
                        mail bcc: '', body: 'Jenkins is unable to deploy into tomcat on prodservers ', cc: '', from: '', replyTo: '', subject: 'Delivery failed', to: 'delivery.team@gmail.com'
                    }
                }
                
                
            }
        }
    }
}
