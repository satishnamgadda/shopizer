pipeline
{
    agent { node { label 'JDK11' } }
    triggers { cron('30 5 * * *') }
    stages {
        stage('vcs') {
            steps {
                git url: 'git@github.com:satishnamgadda/shopizer.git',
                    branch: "release"
            }  
        }
         stage('git') {
            steps {
                sh 'git checkout release',
                sh 'git merge develop --no-ff'
            }
        }
        stage('build') {
            steps {
                sh '/usr/share/maven/bin/mvn package'
            }
        }
        stage('artifacts') {
            steps {
            archiveArtifacts artifacts: 'sm-core/target/*.jar', followSymlinks: false
            }
        }
        stage('results') {
            steps {
            junit 'sm-shop/target/surefire-reports/*.xml'
            }
        }
    }
}