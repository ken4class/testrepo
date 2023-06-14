pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/ken4class/testrepo'
            }
        }
        
        // stage("SonarQube analysis") {
        //    steps {
        //      withSonarQubeEnv('sonarqube') {
        //          sh 'mvn clean package sonar:sonar'
        //      }
        //    }
        // }
        
        // stage('Quality Gate') {
        //     steps {
        //         waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
        //     }
        // }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Publish Test report') {
            steps {
                junit 'target/surefire-reports/TEST-com.example.mywebapp.RegisterServletTest.xml'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
               deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://54.175.201.42:8080')], contextPath: 'demo', war: 'target/RegistrationApp-*.war'
            }
        }
    }
}
