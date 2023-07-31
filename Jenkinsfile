pipeline {
    agent any
    environment {
        BRANCH = "${BRANCH_NAME}"
        APPSYSID = 'ed47cebe97543110cdf8b3e6f053af37'
        CREDENTIALS = 'ServiceNow'
        DEVENV = 'https://dev135601.service-now.com/'
        TESTENV = 'https://dev135601.service-now.com/'
        PRODENV = 'https://dev135601.service-now.com/'
        TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
    }
    stages {
        stage('Build') {
            steps {
                snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
                snPublishApp(credentialsId: "${CREDENTIALS}", url: "${DEVENV}", appSysId: "${APPSYSID}",
                        isAppCustomization: true, obtainVersionAutomatically: true, incrementBy: 2)
            }
        }
        stage('Install') {
            steps {
                snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}", baseAppAutoUpgrade: false)
                snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
            }
        }
        stage('Deploy to Prod') {
            when {
                branch 'master'
            }
            steps {
                snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}", baseAppAutoUpgrade: false)
            }
        }
    }
}