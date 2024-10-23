pipeline 
{
    agent none

    stages
    {
        stage('CLONE GIT REPOSITORY')
        {
            agent
            {
                label 'ubuntu-us-app'
            }
            steps 
            {
                checkout scm
            }
        }

        stage('SCA-SAST-SNYK-TEST')
        {
            agent
            {
                label 'ubuntu-us-app'
            }
            steps 
            {
                echo "SNYK-TEST"
            }
        }

        stage('BUILD-AND-TAG')
        {
            agent
            {
                label 'ubuntu-us-app'
            }
            steps 
            {
                script
                {
                    def app = docker.build("blakemfordham/snake-game")
                    app.tag("latest")
                }
            }
        }

        stage('POST-TO-DOCKERHUB')
        {
            agent
            {
                label 'ubuntu-us-app'
            }
            steps 
            {
                script
                {
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub_credentials")
                    {
                        def app = docker.image("blakemfordham/snake-game")
                        app.push("latest")
                    }
                }
            }
        }

        stage('DEPLOYMENT')
        {
            agent
            {
                label 'ubuntu-us-app'
            }
            steps 
            {
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}
