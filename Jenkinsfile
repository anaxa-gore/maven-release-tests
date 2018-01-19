@Library('apave-shared-library') _

def maven = "M3"
def mavenConfig = "globalMaven"

node('master') {
    stage('Checkout') {
        deleteDir()
        checkout scm
    }

    stage("Build") {
        withMaven(maven: maven, globalMavenSettingsConfig: mavenConfig) {
            sh "mvn clean"
            sh "mvn install -DskipTests"
        }
    }

    stage("Test & Qualité") {
        try {
            // Lancement des tests & du contrôle qualité Sonar
            withMaven(maven: maven, globalMavenSettingsConfig: mavenConfig) {
                sh "mvn test"
                sh "mvn sonar:sonar"
            }
        } finally {
            // Si des tests ont bien été exécutés, on les stocke
            def reports = findFiles(glob: '**/target/surefire-reports/TEST-*.xml');
            if(reports?.size() != 0)
                junit '**/target/surefire-reports/TEST-*.xml'
        }
    }

    stage("Publish") {
        withMaven(maven: maven, globalMavenSettingsConfig: mavenConfig) {
            sh "mvn deploy -DskipTests"
        }
    }

    stage('Release') {
        if(!env.BRANCH_NAME.startsWith("RELEASE_"))
            return;

        milestone();
        def nextReleaseVersion = input(
            message: "Préparation de la prochaine version",
            id: "AskForNextReleaseNumber",
            ok: "OK",
            parameters: [
                [$class: 'StringParameterDefinition',
                 description: 'Entrer le numéro de la prochaine version de développement (X.Y.1).\nPour une branche corrective, laisser vide.',
                 name: 'Numéro de version',
                 defaultValue: null]
            ]);
        milestone();

        // @TODO Check format numéro de version

        try {
            releaseJava(nextReleaseVersion);
        } catch(e) {
            throw new Error("La release a échoué", e);
        }
    }
}
