node {
            stage('SCM') {
                checkout scm
            }
            stage('SonarQube Analysis') {
                def scannerHome = tool 'SonarqubeScanner'
                withSonarQubeEnv() {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
}
