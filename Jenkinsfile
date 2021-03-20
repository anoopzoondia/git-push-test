import java.text.SimpleDateFormat
import groovy.time.TimeCategory
import groovy.time.TimeDuration

node{
    def app

    def source_git = ""
    def destination_git = "https://github.com/anoopzoondia/git-push-test.git"

    if (1==1) {
        def err = null
        currentBuild.result = "SUCCESS"
        env.GIT_COMMIT_MSG = "successfully pushed to repository"

        try {
            stage('Checkout') {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'remote-git-push-test']],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: '9c62d78a-dde8-4447-94e8-b317a4f97dfe', url: 'http://git.zooservers.com:90/anoop/remote-git-push-test.git']]
                ])

                sh 'ls'
             }

             stage ("Moving to sub Directory"){
                dir("remote-git-push-test") {
                    withCredentials([usernamePassword(credentialsId: 'f7483046-b22e-48f6-b6a6-d3c682cbc720',
                      usernameVariable: 'USER',
                      passwordVariable: 'PASS')]) {
                        script {
                            encodedPass=URLEncoder.encode(PASS, "UTF-8")
                        }
                        BRANCH = "master"
                        sh 'git clone https://${USER}:${encodedPass}@${destination_git} -b ${BRANCH}'
                        sh 'git add .'
                        sh 'git commit -m "Commit from CI/CD" '
                        sh 'git push'
                    }
                }
             }


        }catch (caughtError) {
            echo 'exception caught'
            err = caughtError
            currentBuild.result = "FAILURE"
        }finally {
            if (err) {
              throw err
            }
        }


    }
}
