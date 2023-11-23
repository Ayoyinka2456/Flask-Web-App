pipeline {
    agent {
        label 'flask-ansible'
    }
    tools {
        git 'Default'
    }
    stages {
        stage('Echo') {
            steps {
                sh "${env.WORKSPACE}"
            }
        }
        stage('Clone Repository') {
            steps {
                script {
                    // Specify the target directory
                    def targetDir = "${env.WORKSPACE}"
                    // Clone the repository into the specified directory
                    dir(targetDir) {
                        git branch: "main", url: "https://github.com/Ayoyinka2456/Flask-Web-App.git"
                    }
                }
            }
        }
        stage('Configure servers') {
            steps {
                sh "cd ${env.WORKSPACE}/ && ansible-playbook configuration.yml -i hosts.ini"
            }
        }
        stage('Test with test.py file') {

            steps {
                sh "cd ${env.WORKSPACE}/ && ansible-playbook -e test.py deploy.yml -i hosts.ini"
            }
        }
        
        stage('Delay') {
            steps {
                script {
                    sleep(time: 60, unit: 'SECONDS')
                }
            }
        }
        stage('Build the main project.py') {
            steps {
                sh "cd ${env.WORKSPACE}/ && ansible-playbook -e project.py deploy.yml -i hosts.ini"
            }
        }
    }
        post{
            always {
                emailext body: 'Check console output at $BUILD_URL to view the results.', 
                subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', 
                to: 'eas.adeyemi@gmail.com'
            }
        }     
    }
