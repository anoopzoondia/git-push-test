import java.text.SimpleDateFormat
import groovy.time.TimeCategory
import groovy.time.TimeDuration

node{
    def app

    def repo_url = "https://846172205013.dkr.ecr.us-east-1.amazonaws.com"

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
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'CalibrationResults']],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: '9c62d78a-dde8-4447-94e8-b317a4f97dfe', url: 'http://git.zooservers.com:90/anoop/remote-git-push-test.git']]
                ])
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
