pipeline{

    agent {
        label 'windows'
    }
        environment {

            DOCKER_CREDENTIALS_ID= 'dockerhub'
            DOCKER_IMAGE_NAME= 'samadhangapat/inquiryms:latest'
        
           
            DATE = new Date().format("yyyy-MM-dd'T'HH:mm:ss'Z'", TimeZone.getTimeZone('UTC'))
        }
  
        stages {


            stage ('checkout'){

                steps {

                    deleteDir()

                    git url: 'https://github.com/Samraj10/inquiryms.git', branch: 'master'

                }

            }


            stage ('build'){

                steps {

                    bat 'mvn clean package'
                }
            }
            stage ('test'){

                steps {

                    bat 'mvn test'
                }

            }

            stage ('check docker version') {

                steps {

                    script {
                    // Verify Docker is available
                    bat 'docker --version'
                     }
                }

            }

/*
            stage ('build docker image') {

                steps {

                    script {

                        docker.build('samadhangapat/inquiryms:latest')
                    }
                }
            }


            stage('Push Docker Image') {
                steps {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                            docker.image('samadhangapat/inquiryms:latest').push()
                        }
                    }
                }
            }

*/
        stage('SSH into Ansible Server and Run Playbook') {
            steps {
                // Use the SSH Publisher plugin to run commands on the remote server
                sshPublisher( 
                    publishers: [
                        sshPublisherDesc(
                            configName: 'ansible_server',  // Name of the SSH server configured in Jenkins
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/k8s-ims',  // Source files to transfer (optional)
                                    remoteDirectory: '/power-tiller-app',  //                       
                                    execCommand: 'ansible-playbook /home/samra/power-tiller-app/k8s-ims/site.yml',  // Command to execute
                                    removePrefix: '',  // Remove prefix from transferred files (optional)
                                    execTimeout: 120000,  // Execution timeout in milliseconds (optional)
                                    usePty: true  // Use Pseudo Terminal (optional)
                                )
                            ],
                            usePromotionTimestamp: false,
                            useWorkspaceInPromotion: false,
                            verbose: true
                        )
                    ]
                )
            }
        }


    }
}
            