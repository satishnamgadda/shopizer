pipeline {
    agent  { label 'JDK11' }   
    stages {
        stage('vcs') {
            steps {
                   mail subject: 'build started',
                     body: 'build started',
                     to: 'qtdevops@gmail.com'
                git branch: "store", url: 'https://github.com/satishnamgadda/shopizer.git'
            }

        }
        stage('artifactory configuaration') {
            steps {
                rtMavenDeployer (
                   id : "MVN_DEFAULT",
                   releaseRepo : "shop1-libs-release-local",
                   snapshotRepo : "shop1-libs-snapshot-local",
                   serverId : "JFROG-SHOP1"
                )

            }
        }
        stage('Exec Maven') {
            steps {
                rtMavenRun(
                    pom : "pom.xml",
                    goals : "clean install",
                    tool : "mvn",
                    deployerId : "MVN_DEFAULT"
                )
          
            }
        }
    //    stage('Build the Code') {
    //        steps {
    //           withSonarQubeEnv('SONAR') {
    //                sh script: 'mvn clean package sonar:sonar'
    //           }
    //        }
    //    }
        stage('publish build info') {
            steps {
               rtPublishBuildInfo(
                serverId : "JFROG-SHOP1"
               )
            }
        }
    }
    
}