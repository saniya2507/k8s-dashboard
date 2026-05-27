pipeline {
    agent any

    environment {
        NAMESPACE = 'dashboard-monitor'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Grabs the latest code from your repo
                checkout scm
            }
        }

        stage('Prepare Kubernetes Manifest') {
            steps {
                script {
                    // This dynamically injects your index.html into the ConfigMap
                    def htmlContent = readFile('index.html')
                    def yamlContent = readFile('deployment.yaml')
                    
                    // Replaces the placeholder with actual code
                    def updatedYaml = yamlContent.replace('# (Jenkins will automatically inject your index.html here later)', htmlContent)
                    writeFile file: 'manifest.yaml', text: updatedYaml
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Executes the deployment targetting your cluster safely
                sh 'kubectl apply -f manifest.yaml'
            }
        }
    }
}
