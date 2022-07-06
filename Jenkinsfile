pipeline {
    agent any
    tools {
        maven "mvn3"
    }
    stages {
        stage('pull from scm'){
            steps {
                git branch: 'main', credentialsId: 'git-lab-token', url: 'https://github.com/blazerrt86899/chat-app-private.git'
            }
        }
        
        stage('mvn build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            post {
                success {
                    junit '**/target/surefire-reports/*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('checkstyle') {
            steps {
                    sh 'mvn checkstyle:checkstyle'
            }
        }
        
        stage('checkstyle report') {
            steps {
                recordIssues(tools: [checkStyle(pattern: 'target/checkstyle-result.xml')])
            }
        }
        
        stage('code coverage') {
            steps {
                jacoco()
            }
        }
        
        stage('code quality with Sonar') {
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=chatapp -Dsonar.host.url=http://20.231.202.95:9000/ -Dsonar.login=sqa_eb0963c26f4515d4860176b58d840a34ba6a3e49'
            }
        }
        
        stage ('Nexus upload')  {
          steps {
          nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: '40.121.32.216:8081',
          groupId: 'websocket-demo',
          version: '0.0.1-SNAPSHOT',
          repository: 'maven-snapshots',
          credentialsId: 'nexus-repo',
          artifacts: [
            [artifactId: 'websocket-demo',
             classifier: '',
             file: 'target/websocket-demo-0.0.1-SNAPSHOT.jar',
             type: 'jar']
        ]
        )
          }
        }
    
    }
}
