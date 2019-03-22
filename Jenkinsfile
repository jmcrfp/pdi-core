env.RESOLVE_REPO_MIRROR = "https://nexus.pentaho.org/content/groups/omni"

pipeline {
    
    agent { label 'non-master' }
    options { 
        timestamps () 
        timeout(time: 30, unit: 'MINUTES') 
    }
    triggers {
        pollSCM 'H/10 * * * *'
    }
   
    stages {
        stage('Build') {
            steps{
                withMaven(
                    maven: 'maven3-auto',
                    mavenSettingsConfig: '42a0305d-6a85-41ad-a94d-1991589ce3e6',
                    mavenLocalRepo: '.repository') {
                sh "mvn clean install"
                }
            }
        }
        stage('Test') {
            steps{
                withMaven(
                    maven: 'maven3-auto',
                    mavenSettingsConfig: '42a0305d-6a85-41ad-a94d-1991589ce3e6',
                    mavenLocalRepo: '.repository') {
                sh "mvn test"
                }
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
        stage('Archive') {
            steps{
                archive 'target/*.jar'
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
