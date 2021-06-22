def sshArgs = "-o StrictHostKeyChecking=no"
def remoteUser = 'vantonini'
def remoteAddress = '192.168.0.102'
def remotePath = '/home/vantonini'

pipeline {
    agent any 
    stages {
        stage('Remote SSH - SCP') { 
            steps {
                echo "Copying files"
                sshagent(credentials: ['bvg_id']) {
                    sh '''#!/bin/bash
                            ssh $sshArgs $remoteUser@$remoteAddress >> ENDSSH
                            if [ -d $remotePath ]; then
                                echo 'Path already exists'
                            fi
                            echo "$(ip address)"
'''
                    // sh "ssh $sshArgs vantonini@192.168.0.102"
                    // sh "scp -r $WORKSPACE/README.md $remoteUser@$remoteAddress:$remotePath"
                }
            }
        }
        stage('Deploy') { 
            steps {
                echo "Deploy"
            }
        }
    }
}
