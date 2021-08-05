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
        sh 'dotnet run'
      }
    }

    stage('publish') {
      parallel {
        stage('publish') {
          steps {
            sh 'dotnet publish &'
          }
        }

        stage('error') {
          steps {
            sh 'pkill -f dotnet'
          }
        }

      }
    }

    stage('pacakage') {
      steps {
        zip(zipFile: 'pipeline-dotnet-core', archive: true, overwrite: true, dir: '~\\home\\vagrant\\archive')
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