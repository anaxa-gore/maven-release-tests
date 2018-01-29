# Maven roots

Ce projet contient trois sous-projets :

- `root-esb`, utilisé pour les projets TIBCO,
- `root-etl`, utilisé pour les projets Talend,
- `root-java`, utilisé pour les projets Java.

Ces projets doivent être systématiquement utilisés en tant que projet racine 
pour les projets Apave utilisant l'intégration continue. Ils définissent notamment
les répositories Nexus à utiliser pour déployer les artefacts dans chacune des 
trois technologies.