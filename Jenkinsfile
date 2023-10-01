pipeline {
    environment {
        registry = "sin.ocir.io/axf5p8j94qn5/jenkins-oci"
        registryCredential = 'ocid1.containerrepo.oc1.ap-singapore-1.0.axf5p8j94qn5.aaaaaaaakz3ugigmkchgc6cxtqhuhzof3xwb2ewhvmzozo255627uwwgdxeq'
        dockerImage = ''
    }
    agent any
    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/oShyfur/jenkins-oci'
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build(registry + ":${BUILD_NUMBER}")
                }
            }
        }
        stage('Push to OCIR') {
            steps {
                script {
                    docker.withRegistry( 'https://sin.ocir.io', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }
        stage('Cleanup') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}