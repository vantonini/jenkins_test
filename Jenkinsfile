def sshArgs = "-o StrictHostKeyChecking=no"
def remoteUser = 'vantonini'
def remoteAddress = '192.168.0.102'
def remotePath = '/home/vantonini/cpfiles/'

def remote = [:]
remote.name = 'vantonini'
remote.host = '192.168.0.102'
remote.user = 'vantonini'
remote.agent = true
//remote.password = 'password'
remote.allowAnyHosts = true
remote.fileTransfer = 'SCP'

pipeline {
    agent any 
    stages {
        stage('Remote SSH - SCP') { 
            steps {
                echo "Build"
                sshagent(credentials: ['bvg_id']) {
                    // sh "ssh $sshArgs vantonini@192.168.0.102"
                    //sh "scp -r $WORKSPACE/README.md $remoteUser@$remoteAddress:$remotePath"
                    sh "scp -r !(file1.txt file3.txt) $remoteUser@$remoteAddress:$remotePath"
                }
            }
        }
//         stage('Remote SSH - sshPut') {
//             steps {
//                 //writeFile file: 'README.md', text: 'ls -lrt'
//                 sshPut remote: remote, from: 'README.md', into: '/home/vantonini/sshPut/'
//             }
//         }
        stage('Deploy') { 
            steps {
                echo "Deploy"
            }
        }
    }
}
