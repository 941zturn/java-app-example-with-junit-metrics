pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com.cnpmjs.org/jglick/simple-maven-project-with-tests.git'
                git credentialsId: '1846eae3-26e2-4bca-95b9-7f8e5f0be1c5',
                    url: 'http://poc-git.volvocars.com:3000/demo/simple-maven-project-with-tests.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Build Image') {
            steps {
                sh "docker build -t harbor01.volvocars.com/demo/myapp:latest ."
                sh "docker push harbor01.volvocars.com/demo/myapp:latest"
            }
        }
    }
}

