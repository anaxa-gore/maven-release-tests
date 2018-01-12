def maven = "M3"
def mavenConfig = "globalMaven"

node('master') {
    stage('Checkout') {
            checkout scm
    }

    stage('Release') {
        sh 'git checkout develop'
        sh "touch toto.txt"
        sh "git add -A"
        sh "git commit -a -m 'Test !!!'"
        sh "git push"
    }
}