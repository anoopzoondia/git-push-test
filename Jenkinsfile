import java.text.SimpleDateFormat
import groovy.time.TimeCategory
import groovy.time.TimeDuration

node{
    def app

    def source_git = ""
    def destination_git = "github.com/anoopzoondia/git-push-test.git"
    def destination_master_branch = "master"

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

                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'git-push-test']],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: 'f7483046-b22e-48f6-b6a6-d3c682cbc720', url: 'https://github.com/anoopzoondia/git-push-test.git']]
                ])

                sh 'ls'
             }
             stage("Copying content from source to destination"){
                sh "rsync -a --exclude='.git/' --exclude='*' ~/remote-git-push-test/ ~/git-push-test/"
             }

             stage ("Moving to sub Directory"){
                dir("remote-git-push-test") {
                    withCredentials([usernamePassword(credentialsId: 'f7483046-b22e-48f6-b6a6-d3c682cbc720',
                      usernameVariable: 'USER',
                      passwordVariable: 'PASS')]) {



                        sh "git clone https://${USER}:${PASS}@${destination_git} -b master"
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
