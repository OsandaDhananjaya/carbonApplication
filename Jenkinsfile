pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // get the current branch name
                    def branch = env.BRANCH_NAME
                    // check if it is master or test branch
                    if (branch == 'master') {
                        // deploy to dev server
                        sh 'cp target/*.car $MI_HOME/../repository/deployment/server/carbonapps'
                        sh '$MI_HOME/micro-integrator.sh restart'
                    } else if (branch == 'test') {
                        // deploy to test server
                        sh 'cp target/*.car $MI_HOME/../repository/deployment/server/carbonapps'
                        sh '$MI_HOME/micro-integrator.sh restart'
                    } else {
                        // do nothing for other branches
                        echo 'No deployment for branch: ' + branch
                    }
                }
            }
        }
    }
}

