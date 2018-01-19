@Library('apave-shared-library') _

def maven = "M3"
def mavenConfig = "globalMaven"

// On ne build jamais la branche master
if(env.BRANCH_NAME.equals("master"))
    return;

node('master') {
    stage('Checkout') {
        deleteDir();
        checkout scm;
    }

    stage("Build") {
        mvnExecute("mvn clean", maven, mavenConfig);
        mvnExecute("mvn install -DskipTests", maven, mavenConfig)
    }

    stage("Test & Qualité") {
        try {
            // Lancement des tests & du contrôle qualité Sonar
            mvnExecute("mvn test", maven, mavenConfig);
            mvnExecute("mvn sonar:sonar", maven, mavenConfig);
        } finally {
            // Si des tests ont bien été exécutés, on les stocke
            def reports = findFiles(glob: '**/target/surefire-reports/TEST-*.xml');
            if(reports?.size() != 0)
                junit '**/target/surefire-reports/TEST-*.xml'
        }
    }

    stage("Publication Nexus") {
        mvnExecute("mvn deploy -DskipTests", maven, mavenConfig);
    }

    stage("Déploiement Dev") {
        sh 'echo Déploiement'
    }

    stage('Release') {
        if(!env.BRANCH_NAME.startsWith("RELEASE_"))
            return;

        milestone();
        lock(resource:'release', inversePrecedence:true) {
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
        }

        // @TODO Check format numéro de version

        try {
            releaseJava(nextReleaseVersion);
        } catch(e) {
            throw new Error("La release a échoué", e);
        }
    }
}
