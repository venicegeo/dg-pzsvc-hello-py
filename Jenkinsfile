@Library('pipelib@master') _

def THREADFIX_ID = env.THREADFIX_ID ? env.THREADFIX_ID : '115'

node {
  def root = pwd()

  stage('Setup') {
    deleteDir()
    git([
      url: env.GIT_URL ? env.GIT_URL : 'https://github.com/venicegeo/dg-pzsvc-hello-py',
      branch: "master"
    ])
  }

  stage('Archive') {
    sh """
      virtualenv --python=/usr/bin/python3 .env
      . .env/bin/activate
      make -p main.py
      pip install -r requirements.txt
    """
  }

  stage('Staging Deploy') {
    cfPush()
  }

}
