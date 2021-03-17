pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/g0t4/jgsu-spring-petclinic.git', branch: 'main'
            }            
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
                //sh 'false' // true
            }
        
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                     emailext subject: "Job \'${JOB_NAME}\' (build ${BUILD_NUMBER}) ${currentBuild.result}",
                        body: "Please go to ${BUILD_URL} and verify the build", 
                        attachLog: true, 
                        compressLog: true, 
                        to: "test@jenkins",
                        recipientProviders: [upstreamDevelopers(), requestor()]
                }
                //send email anytime the status is changed
                changed {
                    emailext subject: "Job \'${JOB_NAME}\' (build ${BUILD_NUMBER}) ${currentBuild.result}",
                        body: "Please go to ${BUILD_URL} and verify the build", 
                        attachLog: true, 
                        compressLog: true, 
                        to: "test@jenkins",
                        recipientProviders: [upstreamDevelopers(), requestor()]
                        //requestor-> Sends email to the user who initiated the build.
                        //upstreamDevelopers->sends email to the lists of users who comitted changes in the upstram builds that triggered this build.
                        //the above code is generated from the snippets
                }
            }
        }
    }
}
