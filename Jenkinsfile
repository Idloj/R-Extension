node {

  step([$class: 'GitHubSetCommitStatusBuilder'])

  def SBT = "${tool name: 'sbt-0.13.13', type: 'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt"

  checkout scm

  stage('Build and Test') {
    environment {
      R_HOME = '/usr/share/R'
    }
    sh "${SBT} compile"
    sh "${SBT} test"
  }

  post {
    always {
      step([
          $class: 'GitHubCommitStatusSetter',
          errorHandlers: [[$class: 'ShallowAnyErrorHandler']],
          statusResultSource: [
              $class: 'ConditionalStatusResultSource',
              results: [
                  [$class: 'BetterThanOrEqualBuildResult', result: 'SUCCESS', state: 'SUCCESS', message: currentBuild.description],
                  [$class: 'BetterThanOrEqualBuildResult', result: 'FAILURE', state: 'FAILURE', message: currentBuild.description],
                  [$class: 'AnyBuildResult', state: 'FAILURE', message: 'Build status not sucess or failure!']
              ]
          ]
      ])
    }
  }

}
