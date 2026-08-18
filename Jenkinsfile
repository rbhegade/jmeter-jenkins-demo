pipeline {
    agent any

    environment {
        // 🔴 UPDATE THIS to your actual JMeter bin path (use forward slashes / or double backslashes \\)
        JMETER_HOME = 'F:/Rohit/apache-jmeter-5.6.3/bin'
        
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

        stage('Execute JMeter Test') {
            steps {
                echo 'Executing JMeter in Non-GUI mode...'
                // Uses the explicit path to jmeter.bat
                bat "\"${JMETER_HOME}/jmeter.bat\" -n -t tests/test_plan.jmx -l %RESULTS_DIR%/results.jtl -e -o %REPORT_DIR%"
            }
        }
    }

    post {
        always {
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'results/dashboard',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report',
                reportTitles: 'JMeter Performance Report'
            ])

            archiveArtifacts artifacts: 'results/results.jtl', allowEmptyArchive: true
        }
    }
}