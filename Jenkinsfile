node ('') {
    stage('Source') {
        git branch: 'develop',
        credentialsId: 'jenkins_github_access',
        url: 'git@github.com:ldoctori/quiz-cookie-generator.git'
    }
    stage('Build') {
       withMaven{
           sh "mvn clean install"
       }
       env.IMAGE_NAME = "ldoctori/cookie-generator-docker-registry:${env.BUILD_ID}"
       docker.build(env.IMAGE_NAME)
    }

    stage('Deploy to DockerHub') {
        withDockerRegistry(credentialsId: "jenkins_docerhub_access") {
            docker.image(env.IMAGE_NAME).push()
       }
    }

    stage('Clean') {
        cleanWs()
        sh "cd ~/.m2/repository; rm -rf *"
        sh "docker rmi ${env.IMAGE_NAME}"
    }
}