pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                echo "Cloning code"
                git url:"https://github.com/codeForDebugge/hello-world-python.git", branch:"master"
            }
        }
        stage('build image')
        {
            steps
            {
               echo 'Building code'
               sh "docker build -t my-node-app ."
            }
        }
        stage("Push to Docker Hub")
        {
            steps
            {
                echo 'Pushing code to docker hub'
                  withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                    script {
                        // Log in to Docker Hub using credentials
                        sh "docker tag my-node-app ${env.dockerHubUser}/my-node-app:latest"
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker push ${env.dockerHubUser}/my-node-app:latest"
                    }
                  }
            }
        }
        stage('Deploy')
        {
            steps
            {
            
                // Run the new container
                sh "docker-compose down && docker-compose up -d"
                  
            }
            
        }
    }
}
