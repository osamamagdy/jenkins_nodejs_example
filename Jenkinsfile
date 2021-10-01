pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                // Get some code from a GitHub repository
                // Run Maven on a Unix agent.
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {

                sh """
                docker rm -f ${docker ps -a -q}
                docker build . -f dockerfile -t osamamagdy/sprints_jenkins:latest
                docker login -u ${USERNAME}  -p ${PASSWORD}
                docker push osamamagdy/sprints_jenkins:latest
                
                """

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
}
        }
        
        
                stage('CD') {
            steps {

                sh """

                docker run -d -p 3000:3000 osamamagdy/sprints_jenkins:latest
                
                """

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
             post {
                 success {
                     slackSend (color:"#00FF00", message: "notification by script")
                 }
           }
        }
    }
}
