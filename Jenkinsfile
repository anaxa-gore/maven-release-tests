def maven = "M3"
def mavenConfig = "globalMaven"

node('master') {
    stage('Checkout') {
            checkout scm
    }

    stage('Release') {
        sh "touch toto.txt"
        sh "git commit -a -m 'Test !!!'"
    }
}