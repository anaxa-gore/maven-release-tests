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
            message: "Prochaine version de développement (X.Y.1).\nPour une branche corrective, laisser vide.",
            id: "AskForNextReleaseNumber",
            ok: "OK",
            parameters: [
                [$class: 'StringParameterDefinition',
                 description: 'Prochaine version',
                 name: 'nextVersion',
                 defaultValue: null]
            ]);

        // @TODO Check format numéro de version

        try {
            releaseJava(nextReleaseVersion);
        } catch(e) {
            throw new Error("La release a échoué", e);
        }
    }
}
