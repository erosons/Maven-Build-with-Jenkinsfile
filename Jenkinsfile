pipeline  {
    agent none
       /* Jenkins reference materiail -> https://www.jenkins.io/doc/book/pipeline/jenkinsfile/*/
    stages {
         /* Repo cloning */
        stage('repo clone') {
            agent any  /* Specifying on which agent the job should run */
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                git credentialsId: 'git_credential', url: 'https://github.com/erosons/demo.git'
            }
        }
        /* Build stage */
        stage('Buid Step') {
            agent {
                label 'Slave1' /* Specifying on which agent the job should run */
            }
            steps {
                sh 'mvn clean install'
            }
        }
        /* Testing stage with Junit */
        stage('Test') {
             agent {
                label 'Slave2'
             }
            steps {
                /* `make check` returns non-zero on test failures,
                * using sh 'make check || true', if `true` to allow the Pipeline to continue nonetheless
                */
                sh 'make check'
            }
        }
         /* In case of failure */
        post {
             always {
                 junit '**/target/*.xml'
             }
               failure {
                  mail to: me@gmail.com, subject: 'The Pipeline failed :('
               }
        }
        /*Deploymnet phase*/
        stage('Deploy') {
            agent any
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                sh 'make publish'
            }
        }
    }
}