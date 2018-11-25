pipeline {
    agent any

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh 'printenv'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''

                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew check'
                //sh 'echo "Fail!"; exit 1'
                input "Does the staging environment look ok?"
            }
        }
//        stage('Sanity check') {
//            steps {
//                input "Does the staging environment look ok?"
//            }
//        }
    }


properties([
  parameters([
    string(name: 'submodule', defaultValue: 'spring-music'),
    string(name: 'submodule_branch', defaultValue: 'develop')
  ])
])


    post {
        always {
            echo 'This will always run'
//            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
//            junit 'build/reports/**/*.xml'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'This will run only if successful'
            slackSend channel: '#ops-room',
              color: 'good',
              message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
        failure {
            echo 'This will run only if failed'
            mail to: 'lihua.li@cgi.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}

