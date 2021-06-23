def sshArgs = "-o StrictHostKeyChecking=no"
def remoteUser = 'vantonini'
def remoteAddress = '192.168.0.103'
def remotePath = '/home/vantonini/cpfiles/'
def remotePathBackup = '/tmp/nagios_backup/'
def filesToCopy = 'README.md file1.txt'

pipeline {
    agent any 
    stages {
        stage('Copying file over ') { 
                steps {
                    sshagent(credentials: ['vantonini-github']) {
                        sh "scp -rp $filesToCopy $remoteUser@$remoteAddress:$remotePathBackup"
                    }
                }
        }
        stage('Deploy') { 
            steps {
                sshagent(credentials: ['vantonini-github']) {
                    sh """
                        ssh -t $sshArgs $remoteUser@$remoteAddress '''
                        rm -rf $remotePath*
                        cp -p ${remotePathBackup}* $remotePath
                        rm -rf ${remotePathBackup}*
                        '''
                    """
                }
                echo "Deploy"
            }
        }
    }
    post {
    
        success {
            echo 'Success'
            // slackSend(channel: "#dev-ops", tokenCredentialId: "slack-ptr", message: 'Nagios service restarted successfully', color: "good")
        }
        failure {           
            echo 'Failure'
            // slackSend(channel: "#dev-ops", tokenCredentialId: "slack-ptr", message: "Nagios build didn't work. Please check for errors.", color: "danger")
        }
    }
}
