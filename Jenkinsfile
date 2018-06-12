properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage('Build Image') {
        git 'https://github.com/estenrye/dops-final-project.git'
        sh 'docker image build -t training/dops-final-project:latest .'
    }
    stage('Build Tests') {
        sh 'docker image build -t dops-final-project-unittests:latest -f Dockerfile.unittests .'
    }
    stage('Run Tests') {
        sh 'docker container run --rm -it dops-final-project-unittests:latest'
    }
    stage('Push Image to Dev Repository') {
        withCredentials([[$class: 'usernamePassword', usernameVariable: 'USER', credentialsId: 'jenkins DTR user', passwordVariable: 'PASS']]) {
            sh 'docker login --username $USER --password $PASS'
            sh 'docker tag dops-final-project:latest ec2-52-35-77-243.us-west-2.compute.amazonaws.com:4443/finalproject/dev:${env.BUILD_NUMBER}'
            sh 'docker push ec2-52-35-77-243.us-west-2.compute.amazonaws.com:4443/finalproject/dev:${env.BUILD_NUMBER}'
        }
    }
}