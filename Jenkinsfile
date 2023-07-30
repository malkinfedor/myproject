pipeline {
    agent any
    stages {
        stage('GitClone') {
            steps {
                // Get code from a GitHub repository
                git url: 'https://github.com/malkinfedor/myproject.git', 
                 branch: 'ci',
                 credentialsId: 'jenkins-user'
            }
        }
        stage('AddRecordToFile') {
            steps {

                sh "echo \$(date) >> job.log"
                sh "exit 1"
                
            }
        }
        stage("Commit") {
            steps {
                sh('''
                    git branch
                    git config user.name 'my-ci-user'
                    git config user.email 'my-ci-user@users.noreply.github.example.com'
                    git add job.log
                    git commit -m "Update job.log from Jenkins"
                ''')
            }
        }
        stage("Push") {
            environment {
                GIT_AUTH = credentials('jenkins-user')
            }
            steps {
                sh('''
                    echo $GIT_AUTH_USR
                    echo $GIT_AUTH_PSW
                    git config --local credential.helper "!f() { echo username=\\$GIT_AUTH_USR; echo password=\\$GIT_AUTH_PSW; }; f"
                    git push origin HEAD:ci
                ''')
            }
        }
    }
    post {
        always {
            emailext to: "malkinfedor@gmail.com",
            subject: "Jenkins build was ${currentBuild.currentResult}. Job name: ${env.JOB_NAME}",
            body: "Status of ${env.JOB_NAME} job is: ${currentBuild.currentResult}, \nBuild URL: ${env.BUILD_URL}",
            attachLog: true
        }
    }
}
