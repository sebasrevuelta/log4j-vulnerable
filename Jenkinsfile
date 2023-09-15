//It is a global library declared for Semgrep calls
@Library('semgrep') _

pipeline {
  agent any
    environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      SEMGREP_BASELINE_REF = "origin/main"
      SEMGREP_BRANCH = "${BRANCH_NAME}"
      SEMGREP_COMMIT = "${GIT_COMMIT}"
      SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
      SEMGREP_PR_ID = "${env.CHANGE_ID}"
    }
    stages {

      stage ('Generate-LockFile') {
        steps {
            withMaven(maven: 'maven') {
              sh "mvn dependency:tree -DoutputFile=maven_dep_tree.txt"
            }
        }
      }
      
      stage('Semgrep-Scan') {
        steps {
                script {
                    if (env.GIT_BRANCH == 'main') {
                        semgrepFullScan()
                    }  else {
                        sh "git fetch origin +ref/heads/*:refs/remotes/origin/*" 
                        semgrepPullRequestScan()
                    }
                }
            }
        }
    }
}
