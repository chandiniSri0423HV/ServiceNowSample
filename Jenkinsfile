// Example of full CI/CD for customized application on NOW platform

pipeline {
  agent any
//  parameters {
    // ServiceNow Parameters should be created by selecting the option 'This build is parameterized' in job configuration.
    // Otherwise it won't be possible to pass variables between publish -> install steps.
    // snParam(...)
//  }
  environment {
    BRANCH = "${BRANCH_NAME}"
    APPSYSID = 'ed47cebe97543110cdf8b3e6f053af37'
    CREDENTIALS = 'ServiceNow'
    DEVENV = 'https://dev135601.service-now.com/'
    TESTENV = 'https://test135601.service-now.com/'
    PRODENV = 'https://prod135601.service-now.com/'
    TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      steps {
        echo "Build"
        echo "${APPSYSID}"
        echo "${BRANCH}"
        echo "${DEVENV}"
        echo "${CREDENTIALS}"
        
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
