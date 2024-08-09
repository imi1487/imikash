pipeline {
    agent any
    parameters {
        string(name: 'maven_version', defaultValue: '3.9.3', description: 'pass the version of maven')
        string(name: 'terraform_version', defaultValue: '0.15.0', description: 'pass the version of maven')		
    }
    stages {
        stage('download maven') {
            steps {
		sh 'cd /var/lib/jenkins/'
                sh 'sudo wget https://dlcdn.apache.org/maven/maven-3/$maven_version/binaries/apache-maven-$maven_version-bin.tar.gz'
            }
        }
        stage('download terraform') {
            steps {
		sh 'cd /opt'
                sh 'sudo wget  https://releases.hashicorp.com/terraform/$terraform_version/terraform_$terraform_version_linux_amd64.zip'
            }
        }		
   }
}
