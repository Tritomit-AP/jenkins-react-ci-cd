node {
  try {
    stage('Checkout') {
      checkout scm
    }
    stage('Environment') {
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'printenv'
    }
    // stage('Build Docker test'){
    //  sh 'docker build -t react-test -f Dockerfile.test --no-cache .'
    // }
    // stage('Docker test'){
    //   sh 'docker run --rm react-test'
    // }
    // stage('Clean Docker test'){
    //   sh 'docker rmi react-test'
    // }
    stage('Build'){
      if(env.BRANCH_NAME == 'main'){
        sh 'docker build -f Dockerfile.prod -t jenkins-react-app --no-cache .'
        sh 'docker tag jenkins-react-app localhost:5001/jenkins-react-app'
        sh 'docker push localhost:5001/jenkins-react-app'
        sh 'docker rmi -f jenkins-react-app localhost:5001/jenkins-react-app'
      }
    }
        stage('Deploy'){
      if(env.BRANCH_NAME == 'main'){
        sh 'docker pull localhost:5001/jenkins-react-app'
        sh 'docker rm jenkins-react-app-prod -f'
        sh 'docker run -d -p 8888:80 --name jenkins-react-app-prod localhost:5001/jenkins-react-app:latest'
      }
    }
  }
  catch (err) {
    throw err
  }
}
