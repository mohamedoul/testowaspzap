pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                script {
                  startZap(host: "127.0.0.1", port: 9095, timeout: 900, zapHome: "/opt/zaproxy", allowedHosts:['10.0.0.1'], sessionPath:"/path/to/session.session")                }
            }
        }
        stage('Build & Test') {
            steps {
                script {
                    sh "mvn verify -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9091 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=9095" // Proxy tests through ZAP
                }
            }
        }
    }
    post {
        always {
            script {
                archiveZap(failAllAlerts: 1000, failHighAlerts: 1000, failMediumAlerts: 1000, failLowAlerts: 1000, falsePositivesFilePath: "zapFalsePositives.json")
            }
        }
    }
}
