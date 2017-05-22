@Library('pipelib@master') _

def THREADFIX_ID = env.THREADFIX_ID ? env.THREADFIX_ID : '115'

node {
  def root = pwd()

  stage('Setup') {
    git([
      url: env.GIT_URL ? env.GIT_URL : 'https://github.com/venicegeo/dg-pzsvc-hello-py',
      branch: "master"
    ])
  }

  stage('Archive') {
    sh """
      . /opt/rh/rh-python35/enable
      python -m venv .env
      . .env/bin/activate
      make -p vendor
      pip install -d vendor -r requirements.txt
    """
  }

  stage('CI Deploy') {
    cfPush()
    zap {
      threadfixId = THREADFIX_ID
    }
    cfBgDeploy()
  }

  stage('Integration Testing') {
    postman()
  }

  stage('Reset') {
    git([
      url: env.GIT_URL ? env.GIT_URL : 'https://github.com/venicegeo/dg-pzsvc-hello-py',
      branch: "master"
    ])
  }

  stage('Staging Deploy') {
    cfPush {
      cfDomain  = 'TBD'
      cfSpace   = 'TBD'
    }
    cfBgDeploy {
      cfDomain  = 'TBD'
      cfSpace   = 'TBD'
    }
  }

  stage('Cleanup') {
    deleteDir()
  }
}
