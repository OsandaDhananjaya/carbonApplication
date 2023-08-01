pipeline {
    agent any // run the pipeline on any available agent
    stages {
        stage('Build') { // first stage: build the carbon application
            steps {
                sh 'mvn clean install' // run Maven command to build the project
                archiveArtifacts artifacts: 'TestInt2CompositeExporter/target/*.car' // archive the generated car file
            }
        }
        stage('Deploy') { // second stage: deploy the car file to different environments
            parallel { // run the deployment steps in parallel
                stage('Deploy to Dev') { // deploy to dev environment
                    steps {
                        sh '''
                            # copy the car file to the dev server using SSH
                            ssh wso2carbon@dev-server 'mkdir -p /home/wso2carbon/wso2mi-4.2.0/repository/deployment/server/carbonapps/'
                            scp TestInt2CompositeExporter/target/TestInt2CompositeExporter_1.0.0.car wso2carbon@dev-server:/home/wso2carbon/wso2mi-4.2.0/repository/deployment/server/carbonapps/
                        '''
                    }
                }
                stage('Deploy to Test') { // deploy to test environment
                    steps {
                        // copy the car file to the test server using SSH
                        sh '''
                            ssh wso2carbon@test-server 'mkdir -p /home/wso2carbon/wso2mi-4.2.0/repository/deployment/server/carbonapps/'
                            scp TestInt2CompositeExporter/target/TestInt2CompositeExporter_1.0.0.car wso2carbon@test-server:/home/wso2carbon/wso2mi-4.2.0/repository/deployment/server/carbonapps/
                        '''
                    }
                }
            }
        }
    }
}

