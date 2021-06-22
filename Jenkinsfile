def sshArgs = "-o StrictHostKeyChecking=no"
pipeline {
    agent any 
    stages {
        stage('Remote SSH') { 
            steps {
                echo "Build"
                sshagent(credentials: ['bvg_id']) {
                    // sh "ssh $sshArgs vantonini@192.168.0.102"
                    sh "scp $WORKSPACE\README.md vantonini@192.168.0.102:/home/vantonini"
                }
            }
        }
        stage('Test') { 
            steps {
                echo "Test"
            }
        }
        stage('Deploy') { 
            steps {
                echo "Deploy"
            }
        }
    }
}
