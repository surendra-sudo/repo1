# repo1pipeline{
    tools{
        jdk 'myjava'
        maven 'mymaven1'
    }
    agent none
    stages{
        stage('Checkout'){
            agent any
            steps{
                git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
            }
        }
        stage('Compile'){
            agent any
            steps{
                sh 'mvn compile'
            }
        }
        stage('CodeReview'){
            agent any
            steps{
                sh 'mvn pmd:pmd'
            }
        }
        stage('UnitTest'){
            agent {label 'win_slave'}
            steps{
                git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                bat 'mvn test'
            }
        }
        stage('MetricCheck'){
            agent any
            steps{
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
        }
        stage('Package'){
            agent{label 'win_slave'}
            steps{
                bat 'mvn package'
            }
        }
    }
}
