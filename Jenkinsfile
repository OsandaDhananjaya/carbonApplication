pipeline {
    agent any
    environment {
        DEV_MI_SERVER = 'http://localhost:8290'
        TEST_MI_SERVER = 'http://localhost:8300'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' // Assuming your project uses Maven for building
                archiveArtifacts artifacts: 'TestInt2CompositeExporter/target/*.car', onlyIfSuccessful: true
            }
        }
        stage('Deploy to Dev') {
            when {
                branch 'dev' // Change 'dev' to your desired branch name for the dev environment
            }
            steps {
                sh "curl -X POST -F file=@TestInt2CompositeExporter/target/TestInt2CompositeExporter_1.0.0.car ${DEV_MI_SERVER}/carbonapps/"
            }
        }
        stage('Deploy to Test') {
            when {
                branch 'test' // Change 'test' to your desired branch name for the test environment
            }
            steps {
                sh "curl -X POST -F file=@TestInt2CompositeExporter/target/TestInt2CompositeExporter_1.0.0.car ${TEST_MI_SERVER}/carbonapps/"
            }
        }
    }
}

