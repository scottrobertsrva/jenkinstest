pipeline {
   agent any

    environment {
        RUNTIME = sh(returnStdout: true, script: 'date +%Y-%m-%dT%H-%M-%S').trim()
        GIT_COMMIT_MESSAGE = 'Jenkins Git Commit :: $RUNTIME'
        GIT_REPO = 'github.com/scottrobertsrva/jenkinstest'
    }
 
    stages{       

        stage('run backup'){
            steps {
            withCredentials([usernamePassword(credentialsId: 'scott_github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                sh "git checkout master"
                sh "git pull https://${GIT_USERNAME}:${GIT_PASSWORD}@${GIT_REPO}"
            }
            sh "echo $GIT_COMMIT_MESSAGE > test.txt"
         }
        }
        stage('store backup'){
            steps {
                    sh 'git add  test.txt'
                    sh 'git commit -m "commit: $RUNTIME"'
                    withCredentials([usernamePassword(credentialsId: 'scott_github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${GIT_REPO}'
                    }
                }
            }
        }
}
