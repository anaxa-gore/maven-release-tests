@Library('apave-shared-library') _

def maven = "M3"
def mavenConfig = "globalMaven"

node('master') {
    stage('Checkout') {
        deleteDir()
        checkout scm
    }

    stage('Release') {
        if(!env.BRANCH_NAME.startsWith("RELEASE_"))
            return;

        releaseJava("10.3.2");
/**        sshagent(credentials: ['GitlabApave']) {
            sh 'git checkout develop'
            sh 'git pull'
            sh 'git remote -v'
            sh "touch toto.txt"
            sh 'echo "coco" >> toto.txt'
            sh "git add -A"
            sh "git commit -a -m 'Test !!!'"
            sh "git push --all origin"
        }
**/
    }
}
