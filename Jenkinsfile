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

        def nextReleaseVersion = input(
            message: "Prochaine version de développement (correspond à la prochaine release )",
            id: "AskForNextReleaseNumber",
            parameters: [
                [$class: 'StringParameterDefinition',
                 description: 'Prochaine version',
                 name: 'nextVersion']
            ]);

        // @TODO Check format numéro de version

        releaseJava(nextReleaseVersion);
    }
}
