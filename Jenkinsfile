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
      stage('Semgrep-Scan') {
        steps {
                script {
                    if (env.GIT_BRANCH == 'main') {
                        echo "Hello from ${env.GIT_BRANCH} branch"
                        mvn -version
                        mvn dependency:tree -DoutputFile=maven_dep_tree.txt
                        semgrepFullScan()
                    }  else {
                        sh "echo 'Hello from ${env.GIT_BRANCH} branch'"
                        sh "git fetch origin +ref/heads/*:refs/remotes/origin/*" 
                        semgrepPullRequestScan()
                    }
                }
            }
        }
    }
}
