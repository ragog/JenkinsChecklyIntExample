pipeline {
    agent any
    stages {
      
      stages {
        
        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }

        // The interesting part happens here. We use the after_deploy phase to trigger Checkly and either pass or fail the build.
        stage('Trigger Checkly') {
            steps {
                sh 'echo "Deployment finished."'
                // Call Checkly trigger
                sh 'curl "https://api.checklyhq.com/checks/175cdd18-f9ff-4349-bf6f-7735c83528b8/trigger/${CHECKLY_TOKEN}" > ${PWD}/checkly.json'
                // Exit with an error status if we find more than 0 "hasFailures: true" in the output
                // sh 'if [ $(grep -c '"hasFailures":true' ${PWD}/checkly.json) -ne 0 ]; then exit 1; fi'
                sh 'if [ $(grep -c "hasFailures":true ${PWD}/checkly.json) -ne 0 ]; then exit 1; fi'
            }
        }
      }
    }
}
