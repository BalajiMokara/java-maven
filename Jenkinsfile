properties([parameters([choice(choices: ['master', 'main', 'develop'], description: 'select the branch name', name: 'Branch')])])

pipeline {
    agent any

    stages {
        stage('Step1 : Git Clone') {
            steps {
                git branch: "${params.Branch}",
                credentialsId: 'my_git_cred',
                url: 'https://github.com/BalajiMokara/java-maven.git'
            }
        }
        stage('step 2: maven build'){
            steps{
                script{
                def MVN_PATH = tool (name: 'maven381', type: 'maven') + "/bin/mvn"
                sh "$MVN_PATH package"
                }
            }
        }
        stage('step:3 sonar scanner'){
            steps{
                script{
                def Sonar_path = tool name: 'sonar462', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                
                }
            }
        }
        stage('step :4 docker image build'){
            steps{
                sh "docker build -t balaji22827/my-java-app:v1.0 ."
            }
        }
        stage('step:5 docker image push'){
            steps{
                withCredentials([string(credentialsId: 'docker_cred', variable: 'DockerPassword')]) {
                    sh "docker login -u balaji22827 -p ${DockerPassword}"
                    sh "docker push balaji22827/my-java-app:v1.0"
                }
               
            }
        }
       
    }
}

