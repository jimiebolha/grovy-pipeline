#!groovy

node('nodejs') {

    currentBuild.result = "SUCCESS"

    try {

       stage('Checkout'){
          checkout scm
       }

       stage('Build'){
          sh 'npm install'
       }

       stage('Test'){
         env.NODE_ENV = "test"
         sh 'npm run ci-test'
       }

       stage('Deploy'){
         sh 'docker build -t myapp . &amp;&amp; docker run -d myapp'
       }

       stage('Cleanup'){
         sh 'npm prune'
         sh 'rm node_modules -rf'
         slackNotify channel: '#ci-nodejs', message: 'pipeline successful: myapp'
       }
    }
    catch (err) {

        currentBuild.result = "FAILURE"
        slackNotify channel: '#ci-nodejs', message: 'pipeline failed: myapp'
        throw err
    }

}
