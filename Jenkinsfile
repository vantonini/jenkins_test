def sshArgs = "-o StrictHostKeyChecking=no"
def remoteUser = 'vantonini'
def remoteAddress = '192.168.0.102'
def remotePath = '/home/vantonini/cpfiles/'
def remotePathBackup = '/tmp/nagios_backup/'
def filesToCopy = 'README.md file1.txt'

pipeline {
    agent any 
    stages {
        stage('Copying file over ') { 
                steps {
                    sshagent(credentials: ['bvg_id']) {
                        script {
                            try {
                                sh "scp -rp $filesToCopy $remoteUser@$remoteAddress:$remotePathBackup"
                            }
                            catch (err) {
                               echo err
                            }
                        }
                    }
                }
            }
        }
        stage('Deploy') { 
            steps {
                sshagent(credentials: ['bvg_id']) {
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
}
