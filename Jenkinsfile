def maven = "M3"
def mavenConfig = "globalMaven"

node('master') {
    stage('Checkout') {
            deleteDir()
            checkout scm
    }

    stage('Release') {
        sshagent(credentials: ['root']) {
            sh 'git checkout develop'
            sh 'git pull'
            sh 'git remote -v'
            sh "touch toto.txt"
            sh "git add -A"
            sh "git commit -a -m 'Test !!!'"
            sh "git push -a origin"
        }
    }
}