pipeline {
    agent any
    stages {
        stage("Stage 1 -Source"){
            steps {
                echo "Githubからcheckout"
                echo "PushしたらCIが回るか検証3"
                git url: 'https://github.com/Sato-2525/jenkins-sample.git', branch: 'main', credentialsId: '4e6b1826-1552-497c-ba9a-232595a4e5ff'
                // Change file permisson
                sh "ls -la maya_sato_hello"
            }
        }
        stage("Stage 2 -Build"){
            steps {
                sh'''
                python3 -m venv venv
                chmod +x venv/bin/activate
                venv/bin/activate
                source /var/lib/jenkins/.bashrc
                pip list
                pip install -r requirements.txt
                '''
            }
        }
        stage("Stage 3 -Testing"){
            steps {
                sh "python3 -m pytest testing/"
            }
        }
        stage("Stage 4 -Deploy（AWS SAM Lambda関数を単体でデプロイ）"){
            steps {
                echo "Sam Build"
                echo "Sam Deploy"
            }
        }
    }
    post {
        always {
            script {
                def slackToken = null
    
                // クレデンシャルを取得
                withCredentials([string(credentialsId: 'slack-token', variable: 'slackToken')]) {
                    slackSend(
                        token: slackToken,
                        channel: '#maya_bot',
                        color: currentBuild.resultIsBetterOrEqualTo('SUCCESS') ? 'good' : 'danger',
                        message: "Build ${currentBuild.result}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'\n${env.BUILD_URL}"
                    )
                }
            }
        }
    }
}  