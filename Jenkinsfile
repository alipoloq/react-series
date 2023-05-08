pipeline{
    agent any
    tools{
        nodejs 'nodejs'
    }
    parameters{
        string(name: 'container_name', defaultValue: 'pagina_web', description: 'Nombre del contenedor de docker')
        string(name: 'image_name', defaultValue: 'pagina_img', description: 'Nombre de la imagen docker')
        string(name: 'tag_name', defaultValue: 'lts', description: 'Tag de la imagen de la página')
        string(name: 'container_port', defaultValue: '8080', description: 'Puerto que usa el contenedor')
    }
    stages{
        stage('install'){
                steps{
                    git branch: 'main', url: 'https://github.com/alipoloq/react-series.git'
                    dir('react-series'){
                    sh 'npm install'
                }
            }
        }
        stage('test'){
            steps{
                dir('react-series'){
                    sh 'npm run test'
                }
            }
        }
        stage('build'){
            steps{
                dir('react-series'){
                    script{
                        try{
                            sh 'docker stop ${image_name}'
                            sh 'docker rm ${container_name}'
                            sh 'docker rmi ${image_name}:${tag_image}'
                        } catch (Exception e){
                            echo 'Exception occurred: '+ e.toString()
                        }
                    }
                    sh 'npm run build'
                    sh 'docker build -t ${image_name}:${tag_name} .'
                }
            }
        }
        stage('deploy'){
            steps{
                sh 'docker run -d -p ${container_port}:8080 --name ${container_name} ${image_name}:${tag_image}'
            }
        }
    }
}