pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                script {
                    startZap(host: "127.0.0.1", port: 9089,  zapHome: "/usr/local/bin/ZAP_2.7.0/", sessionPath:"var/lib/jenkins/workspace/test_owaspzap_pipeline/session.session", allowedHosts:['github.com']) // Start ZAP at /opt/zaproxy/zap.sh, allowing scans on github.com (if allowedHosts is not provided, any local addresses will be used
                }
            }
        }
        stage('Build & Test') {
            steps {
                script {
                    sh "mvn verify -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9089 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=9089" // Proxy tests through ZAP
                }
            }
        }
    }
    post {
        always {
            script {
                archiveZap(failAllAlerts: 100000, failHighAlerts: 10000, failMediumAlerts: 10000, failLowAlerts: 10000, falsePositivesFilePath: "zapFalsePositives.json")
            }
        }
    }
}
