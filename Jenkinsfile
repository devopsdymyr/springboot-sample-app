properties properties: [
  [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '30', numToKeepStr: '10']],
  disableConcurrentBuilds()
]

node {
  def buildUrl = env.BUILD_URL
  def buildNumber = env.BUILD_NUMBER
  def workspace = env.WORKSPACE
  def mvnHome = tool 'Maven'
  env.JAVA_HOME = tool 'JDK8'
  env.PATH = "${env.JAVA_HOME}/bin:${mvnHome}/bin:${env.PATH}"

  // PRINT ENVIRONMENT TO JOB
  echo "workspace directory is $workspace"
  echo "build URL is $buildUrl"
  echo "build Number is $buildNumber"
  echo "PATH is $env.PATH"

  stage('Clean workspace') {
    deleteDir()
  }

  stage('Checkout') {
    checkout scm
  }

  stage('Build & Test') {
    sh "./mvnw clean install"
  }

  stage('Deploy') {
    sh "./mvnw -Paws deploy -DskipTests=true -DskipITs=true -DskipUnitTests=true -Dbeanstalk.cnamePrefix=packt-spring-env-test.b4dytngfpm.us-east-1"
  }
}
