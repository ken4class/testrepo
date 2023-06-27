pipeline {
    agent any

    stages {
        stage('clone the code') {
            steps {
               // sh "echo Hello Team, This is femlab project for year 2023"
               // sh "The project will last for three weeks and two days"
             git branch: 'main', url: 'https://github.com/gbadamosisaheedo/testrepo'
            }
        }
        
        stage('UNit Test') {
            steps {
             sh 'mvn test'
            }
        }
        
        stage('Publish Test report') {
            steps {
             junit 'target/surefire-reports/TEST-com.example.mywebapp.RegisterServletTest.xml'
            }
        }
        
        stage('SonarQube code Analysis') {
            steps {
        withSonarQubeEnv('sonar_jenkins') {
                   sh 'mvn clean package sonar:sonar'
                }
           }
        }
        
        // stage('Quality Gate') {
        //    steps {
        //    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_jenkins'
        //   }
        //}
        
        stage('Build Artifact') {
            steps {
             sh 'mvn clean package'
            }
        }
        
        stage('Artifact deploy to nexus') {
            steps {
             nexusArtifactUploader artifacts: [[artifactId: 'RegistrationApp', 
             classifier: '', file: 'target/RegistrationApp-1.2.war', 
             type: 'war']], 
             credentialsId: 'nexus', 
             groupId: 'com.example', 
             nexusUrl: '52.90.175.198:8081',
             nexusVersion: 'nexus3', 
             protocol: 'http', 
             repository: 'my-repo', 
             version: 'v1.3' 
            }
        }
        
        stage('Deploy to tomcat') {
            steps {
             deploy adapters: [tomcat9(credentialsId: 'home', path: '', url: 'http://52.23.240.149:8080')], contextPath: 'reg', war: '**/*.war'
            }
        }
        
         stage('Notification Mail to the Team') {
            steps {
             emailext body: '''Hi Team,

This is to inform you all that the build was successful and we can move to second project in our schedules.

Regards, ''', recipientProviders: [buildUser()], subject: 'The build was successful ', to: 'gbadamosisaheedo@gmail.com'
            }
        }
    }
}