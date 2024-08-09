pipeline {
    agent any

    // Define parameters for the pipeline
    parameters {
        booleanParam(name: 'INSTALL_MAVEN', defaultValue: true, description: 'Install Maven?')
        booleanParam(name: 'INSTALL_JENKINS', defaultValue: true, description: 'Install Jenkins?')
        booleanParam(name: 'INSTALL_DOCKER', defaultValue: true, description: 'Install Docker?')
    }

    stages {
        stage('Install Maven') {
            when {
                expression { return params.INSTALL_MAVEN }
            }
            steps {
                script {
                    echo 'Installing Maven...'
                    sh '''
                        sudo apt-get update
                        sudo apt-get install -y maven
                        mvn -version
                    '''
                }
            }
        }

        stage('Install Jenkins') {
            when {
                expression { return params.INSTALL_JENKINS }
            }
            steps {
                script {
                    echo 'Installing Jenkins...'
                    sh '''
                        wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
                        sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
                        sudo apt-get update
                        sudo apt-get install -y jenkins
                        sudo systemctl start jenkins
                        sudo systemctl enable jenkins
                    '''
                }
            }
        }

        stage('Install Docker') {
            when {
                expression { return params.INSTALL_DOCKER }
            }
            steps {
                script {
                    echo 'Installing Docker...'
                    sh '''
                        sudo apt-get update
                        sudo apt-get install -y \
                            ca-certificates \
                            curl \
                            gnupg \
                            lsb-release
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
                        echo \
                          "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
                          $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
                        sudo apt-get update
                        sudo apt-get install -y docker-ce docker-ce-cli containerd.io
                        sudo systemctl start docker
                        sudo systemctl enable docker
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
    }
}
