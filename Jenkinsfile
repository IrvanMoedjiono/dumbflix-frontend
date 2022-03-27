def secret = 'server'
def server = 'ubuntu@10.224.143.122'
def dir = 'dumbflix-frontend'
def branch = 'master'

pipeline{
	agent any
	stages{
		stage ('Delete container and images & git pull'){
			steps{
				sshagent([secret]) {
					sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
					cd ${dir}
					docker-compose down 
					docker system prune -f
					git pull origin ${branch}
					exit
					EOF"""
				}
			}
		}
	stage ('Build Images'){
                        steps{
                                sshagent([secret]) {
                                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                        cd ${dir}/nginx
					docker build -t nginx:re .
					cd ..
                                        docker-compose build
                                        exit
                                        EOF"""
                                }
                        }
                }
	stage ('Deploy Container'){
                        steps{
                                sshagent([secret]) {
                                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                        cd ${dir}
                                        docker-compose up -d
                                        exit
                                        EOF"""
                                }
                        }
                }

	}
}
