pipeline {
    options{
        timeout(time:1,unit:'HOURS')
    }
    environment {
        docker_image_name = "java8-maven3-junit5"
    }
    
    agent {
    dockerfile {
        additionalBuildArgs '--no-cache=true --build-arg "JENKINS_USER_ID=112" --build-arg "JENKINS_GROUP_ID=117"'
        args '-v ${PWD}/.m2:/usr/share/maven/.m2'
        dir '.'
        filename 'Dockerfile'
        label env.docker_image_name
        }
    }
    stages {
        stage('maven execution') {
            steps {
                script {
                    dir('.') {
                        sh 'set HTTP_PROXY=$HTTP_PROXY'
                        sh 'set HTTPS_PROXY=$HTTP_PROXY'
                        sh 'mvn clean package site'
                    }
                }
            }
        }
        stage('Analysis') {
          environment {
    resultPath = "TEST.xml"
    checkstyleReport = "checkstyle.xml"
    findbugsReport = "findbugs.xml"
             }
            steps {
                script {
                    dir('.') {
                        sh 'echo "Analysis stage"'
                        sh "mvn clean test checkstyleMain findbugsMain"
                    }
                      post {
    success {
      junit resultPath
      recordIssues tool: checkStyle(pattern: checkstyleReport)
      recordIssues tool: findBugs(pattern: findbugsReport)
                }
            }
        }

    }
  }
}
    }