pipeline {
    agent any

    environment {
        // Path configuration
        RESULTS_DIR = 'results'
        REPORT_DIR  = 'results/dashboard'
    }

    stages {
        stage('Clean Old Results') {
            steps {
                bat '''
                    if exist results rmdir /s /q results
                    mkdir results
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                // Jenkins automatically checks out the repository defined in the job
                echo 'Checking out latest code from GitHub...'
            }
        }

        stage('Execute JMeter Test') {
            steps {
                echo 'Executing JMeter in Non-GUI mode...'
                // -n: Non-GUI mode
                // -t: Test plan location
                // -l: Test log results file (.jtl)
                // -e -o: Generate HTML dashboard report
                bat "jmeter -n -t tests/test_plan.jmx -l %RESULTS_DIR%/results.jtl -e -o %REPORT_DIR%"
            }
        }
    }

    post {
        always {
            // Publish the JMeter HTML Dashboard
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'results/dashboard',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report',
                reportTitles: 'JMeter Performance Report'
            ])

            // Archive the raw .jtl file for historical metrics
            archiveArtifacts artifacts: 'results/results.jtl', allowEmptyArchive: true
        }
    }
}