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
        withMaven(maven:maven, mavenSettingsConfig:mavenConfig) {
            sh "mvn deploy -DskipTests -e -X"
        }
        //mvnExecute("mvn deploy -DskipTests -e -X", maven, mavenConfig);
    }

    stage("Déploiement Dev") {
        sh 'echo Déploiement'
    }
}

if(env.BRANCH_NAME.startsWith("RELEASE_")) {
    milestone();
    def nextReleaseVersion = null;
    /*def startRelease = false;
    //lock(resource:'release', inversePrecedence:true) {
        timeout(time: 2, unit:'MINUTES'){
             startRelease = input(
                message: "Déclencher la release ?",
                id: "startRelease",
                ok: 'Oui',
                parameters: [
                    [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
                    ])
                );
        }

        if(!startRelease)
            return;
*/
         nextReleaseVersion = input(
            message: "Préparation de la prochaine version",
            id: "AskForNextReleaseNumber",
            ok: "OK",
            parameters: [
                [$class: 'StringParameterDefinition',
                 description: 'Entrer le numéro de la prochaine version de développement (X.Y.1).\nPour une branche corrective, laisser vide.\nCliquer sur annuler pour repousser la release.',
                 name: 'Numéro de version',
                 defaultValue: null]
            ]);
    //}
    milestone();

    node('master') {
        stage('Release') {
            // @TODO Check format numéro de version
            try {
                releaseJava(nextReleaseVersion);
                mvnExecute("mvn deploy -DskipTests", maven, mavenConfig);
            } catch(e) {
                throw new Error("La release a échoué", e);
            }
        }
    }
}
