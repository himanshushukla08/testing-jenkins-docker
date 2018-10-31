pipeline {
  agent any
  environment {
    MAJOR_VERSION = 1
  }
  stages {
    stage('Unit Tests') {
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
    stage('publish') {
      steps {
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
    stage('Test on CentOS') {
      agent {
        docker 'fabric8/java-centos-openjdk8-jdk:1.4.0'
      }
      steps {
        sh "curl $JENKINS_IP/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar -o rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage('Test on debian') {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "curl $JENKINS_IP/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar -o rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage('code promotion') {
      steps {
        sh "cp /var/www/html/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}".jar /var/www/html/rectangles/all/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}".jar
      }
    }
  }
}
