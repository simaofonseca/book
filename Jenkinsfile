pipeline {
    agent any

        parameters { 
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'backend java', description: 'Docker Image')
        string(name: 'DOCKER_CONTAINER_NAME', defaultValue: 'backend java', description: 'Docker Container Name')
        string(name: 'DOCKER_PORT', defaultValue: '3000', description: 'Docker Port')

    } 

        tools {
        maven "Maven"
    	}

        stage ('CleanResources') {
            agent any
            steps
            {
                cleanWs()
            }
        }

        stage ('Build Maven') {
            steps {
                withMaven {maven: 'MAVEN_HOME') {
                    sh "mvn package -DskipTests=true"
                } 
            }
        }   

        stage ('Build Docker Image') {
                agent any
                steps {
                        sh 'docker build -t "${DOCKER_IMAGE_NAME}" .'
                }
            }

            stage ('Run Docker Container') {
                agent any
                steps {
                    sh 'docker rm -f "${DOCKER_IMAGE_NAME}"'s
                    sh 'docker run -d -p ${DOCKER_PORT}:3000 --name "${DOCKER_CONTAINER_NAME}" "${DOCKER_IMAGE_NAME}"'
                }
            }
        }
    }
}