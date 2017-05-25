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
      python3 -m venv --without-pip .env
      . .env/bin/activate
      make -p main.py
      pip install -r requirements.txt
    """
  }

  stage('CI Deploy') {
    cfPush()
  }

}
