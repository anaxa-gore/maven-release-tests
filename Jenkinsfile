@Library('apave-shared-library') _

import com.apave.jenkins.deploy.*;

class DevDeployer implements Deployer {
    boolean deploy(){
        sh 'echo CA MARCHE !!'
    }
}

def devDeployer = new DevDeployer();
buildJava(devDeployer);