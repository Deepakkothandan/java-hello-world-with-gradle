#!groovy

node {

  def server = Artifactory.server 'my-artifactory'
  def rtGradle = Artifactory.newGradleBuild()
  def buildInfo = Artifactory.newBuildInfo()

  stage ("checkout scm") {
    checkout scm
  }

  stage ('Artifactory / Build Info configuration') {
    // rtGradle.tool = 'Gradle421' // Tool name from Jenkins configuration
    rtGradle.useWrapper = true
    rtGradle.deployer repo:'master-artifacts',  server: server
    // rtGradle.resolver repo:'gradle-resolver-virtual', server: server
    rtGradle.usesPlugin = false // Artifactory plugin already defined in build script

    buildInfo.env.capture = true
  }

  stage ('Exec Gradle') {
    rtGradle.run buildFile: 'build.gradle', tasks: 'clean build artifactoryPublish', buildInfo: buildInfo
  }

  stage ('Publish build info') {
    server.publishBuildInfo buildInfo
  }
}
