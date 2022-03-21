node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'
    // Get some code from a GitHub repository
    checkout scm
    // Note: if this is copy and pasted into pipeline script, the following will work while the above handles branches and such
    // git url: 'https://github.com/saucelabs-sample-test-frameworks/Java-TestNG-Selenium.git'

    stage 'Compile'
        sh "export MAVEN_HOME=/opt/maven"
        sh "export PATH=$PATH:$MAVEN_HOME/bin"
        sh "mvn --version"
        sh "mvn clean package"
        sh "mvn compile"
        stage 'Test'
        sauce('saucelabs') {
            sauceconnect(useGeneratedTunnelIdentifier: true, verboseLogging: true) {
                sh "mvn test"
            }
        }

    stage 'Collect Results'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    step([$class: 'SauceOnDemandTestPublisher'])
}
