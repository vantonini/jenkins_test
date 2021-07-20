def sshArgs = "-o StrictHostKeyChecking=no"
def remoteUser = 'vantonini'
def remoteAddress = '192.168.2.102'
def remotePath = '/home/vantonini/cpfiles/'
def remotePathBackup = '/tmp/nagios_backup/'
def remotePathTemp = '/tmp/nagios_path_temp/'
def filesToCopy = 'README.md file1.txt'

pipeline {
    agent any 
    stages {
        stage('Copying file over ') { 
            steps {
                sshagent(credentials: ['vantonini-github']) {
                        sh """
                            ssh -t $sshArgs $remoteUser@$remoteAddress "[ ! -d ${remotePathTemp} ] && mkdir -p ${remotePathTemp}; [ ! -d ${remotePathBackup} ] && mkdir -p ${remotePathBackup}"
                            scp -rp $filesToCopy $remoteUser@$remoteAddress:$remotePathBackup
                        """
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
            
        }
        failure {           
            echo 'Failure'

        }
    }
}
