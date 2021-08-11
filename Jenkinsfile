pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('git checkout') {
      steps {
        git(url: 'https://github.com/davidvr1/pipelines-dotnet-core.git', branch: 'master', changelog: true, poll: true, credentialsId: 'github')
      }
    }

    stage('build') {
      steps {
        sh 'dotnet build'
      }
    }

    stage('run the app') {
      steps {
        sh 'dotnet run & dotnet publish'
      }
    }

    stage('publish') {
      steps {
        sh 'dotnet publish &'
      }
    }

    stage('pacakage') {
      steps {
        zip(zipFile: 'dotnetecorepipeline.zip', archive: true, overwrite: true)
        cleanWs(cleanWhenSuccess: true)
      }
    }

    stage('nitification') {
      steps {
        slackSend(channel: 'david-varshoer', message: 'success build .net core')
      }
    }

  }
}