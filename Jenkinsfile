pipeline {
    agent any

    environment {
        docker_registry = "phpstacktest"
        server_registry = "phpstacktest/accountill-server"
        client_registry = "phpstacktest/accountill-client"
        registryCredential = 'dockerhub'
        dockerImage = ''

        // DOCKER_BUILDKIT = 1 // Experimental faster build system
        // REPO_NAME = "ever"
        // IMAGE_CORE_API = "ever-core"
        // IMAGE_ADMIN_WEB_ANGULAR = "ever-admin-web-angular"
        // GITHUB_DOCKER_USERNAME = credentials('github-docker-username')
        // GITHUB_DOCKER_PASSWORD = credentials('github-docker-password')
        // GITHUB_DOCKER_REPO = "docker.pkg.github.com/ever-co/ever"
        // GITHUB_DISPATCH_TOKEN = credentials('github-dispatch-token')
        // GITHUB_TOKEN = credentials('github-token')
        // CI_URL = "ci.ever.co"
        // AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        // AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        // AWS_DEFAULT_REGION = "us-east-1"
        // AWS_ECR_REPO = """${sh(
        //     script: "aws ecr get-login --no-include-email --region ${env.AWS_DEFAULT_REGION} | grep -o 'https://.*' | sed -e 's|https://||g' | tr -d '\n'",
        //     returnStdout: true
        // )}"""
        // AWS_ECR_PASSWORD = """${sh(
        //     script: "aws ecr get-login-password --region ${env.AWS_DEFAULT_REGION}",
        //     returnStdout: true
        // )}"""
        // WORKFLOW_ID = """${sh(
        //     script: "curl --silent -X GET -H 'Accept: application/vnd.github.v3+json' https://api.github.com/repos/ever-co/$REPO_NAME-pulumi/actions/workflows/pulumi.yml | tr -s ' ' | tr -d '\r' | tr -d '\n' | grep -Eo '[0-9]{5,9}' | uniq",
        //     returnStdout: true
        // )}"""
    }

    stages {
        // stage("Clone") {
        //     steps{
        //         git branch: 'develop',
        //             url: 'https://github.com/ever-co/ever.git'
                
        //         sh """
        //             curl 'https://api.github.com/repos/ever-co/${REPO_NAME}/statuses/$GIT_COMMIT' -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Content-Type: application/json' -X POST -d '{"state": "pending", "context": "Jenkins", "description": "Jenkins pipeline is running", "target_url": "https://$CI_URL/job/${JOB_NAME}/$BUILD_NUMBER/console"}'
        //         """
        //     }
        //     post {
        //         success {
        //             echo "Cloning successful..."
        //         }
        //         failure {
        //             echo "Cloning failed! See log for details. Terminating..."
        //         }
        //     }
        // }
        // stage('Build'){
        //     parallel {
                stage("Build Server package") {
                    steps {
                        bat "npm cache clean --force && cd server && npm i --prod && cd .."
                    }
                    post{
                        success {
                            echo "Server package node modules installed!"
                        }
                        failure {
                            echo "Server package node modules failed..."
                        }
                    }
                }
                stage("Build Client package Image") {
                    steps {
                        bat "npm cache clean --force && cd client && npm i --prod && cd .."
                    }
                    post {
                        success {
                            echo "Client package node modules installed!"
                        }
                        failure {
                            echo "Client package node modules failed..."
                        }
                    }
                }
        //     }
        // }
        stage("Docker Image Build") {
            parallel {
                stage("Server Image") {
                    steps {
                        bat "docker build -t ${env.server_registry}:$BUILD_NUMBER -f server/Dockerfile ."
                    }
                    post{
                        success {
                            echo "Image for Server built!"
                        }
                        failure {
                            echo "Server Image build failed..."
                        }
                    }
                }
                stage("Client Image") {
                    steps {
                        bat "docker build -t ${env.client_registry}:$BUILD_NUMBER -f client/Dockerfile ."
                    }
                    post{
                        success {
                            echo "Image for Client built!"
                        }
                        failure {
                            echo "Client Image build failed..."
                        }
                    }
                }
            }
        }
        // stage("Login to repositories") {
        //     steps {
        //         sh "docker login docker.pkg.github.com -u ${GITHUB_DOCKER_USERNAME} -p ${GITHUB_DOCKER_PASSWORD}"
        //         sh "docker login ${AWS_ECR_REPO} -u AWS -p ${AWS_ECR_PASSWORD}" // Login to AWS ECR
        //     }
        // }
        // stage ("Push To AWS ECR") {
        //     parallel {
        //         stage ("Core API Image") {
        //             steps {
        //                 // Tag and push to ECR
        //                 sh "docker tag ${IMAGE_CORE_API} ${AWS_ECR_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker push ${AWS_ECR_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker rmi ${AWS_ECR_REPO}/${IMAGE_CORE_API}" // Cleans tag
        //             }
        //             post {
        //                 success {
        //                     echo "Successfuly pushed to ECR on build ${env.BUILD_ID}!"
        //                 }
        //                 failure {
        //                     echo "Push to ECR failed! See log for details..."
        //                 }
        //             }
        //         }
        //         stage ("Admin Web Angular Image") {
        //             steps {
        //                 sh "docker tag ${IMAGE_ADMIN_WEB_ANGULAR} ${AWS_ECR_REPO}/${IMAGE_ADMIN_WEB_ANGULAR}"
        //                 sh "docker push ${AWS_ECR_REPO}/${IMAGE_ADMIN_WEB_ANGULAR}"
        //                 sh "docker rmi ${AWS_ECR_REPO}/${IMAGE_ADMIN_WEB_ANGULAR}" // Cleans tag
        //             }
        //             post {
        //                 success {
        //                     echo "Successfully pushed to ECR on build ${env.BUILD_ID}!"
        //                 }
        //                 failure {
        //                     echo "Push to ECR failed! See log for details..."
        //                 }
        //             }
        //         }
        //     }
        // }
        
        // stage ("Push to GitHub") {
        //     parallel {
        //         stage ("Core API Image") {
        //             steps {
        //                 sh "docker tag ${IMAGE_CORE_API} ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker push ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker rmi ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //             }
        //             post {
        //                 success {
        //                     echo "Successfully pushed to GitHub on build ${env.BUILD_ID}!"
        //                 }
        //                 failure {
        //                     echo "Push to GitHub failed! See log for details..."
        //                 }
        //             }
        //         }
        //         stage ("Admin Web Angular Image") {
        //             steps {
        //                 sh "docker tag ${IMAGE_CORE_API} ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker push ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //                 sh "docker rmi ${GITHUB_DOCKER_REPO}/${IMAGE_CORE_API}"
        //             }
        //             post {
        //                 success {
        //                     echo "Successfully pushed to GitHub on build ${env.BUILD_ID}!"
        //                 }
        //                 failure {
        //                     echo "Push to GitHub failed! See log for details..."
        //                 }
        //             }
        //         }
        //     }
        // }
        
        stage('Building Server image') {
            steps{
              script {
                dockerImage = docker.build server_registry + ":$BUILD_NUMBER"
              }
            }
        }
        
        stage('Deploy Server Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }

        stage('Remove Unused docker server image') {
          steps{
            bat "docker rmi $server_registry:$BUILD_NUMBER"
          }
        }

        stage('Building Client image') {
            steps{
              script {
                dockerImage = docker.build client_registry + ":$BUILD_NUMBER"
              }
            }
        }
        
        stage('Deploy Client Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }

        stage('Remove Unused docker client image') {
          steps{
            bat "docker rmi $client_registry:$BUILD_NUMBER"
          }
        }

        // stage ("Pulumi Update") {
        //     steps {
        //         sh """
        //             curl -sX POST 'https://api.github.com/repos/ever-co/${REPO_NAME}-pulumi/actions/workflows/2319005/dispatches' -H 'Accept: application/vnd.github.v3+json' -H 'Authorization: token ${GITHUB_DISPATCH_TOKEN}' -d '{"ref": "master"}'
        //         """
        //     }
        // }
    }
    post {
        success {
            echo "Ever CI/CD pipeline executed successfully!"
            // sh """
            //     curl 'https://api.github.com/repos/ever-co/${REPO_NAME}/statuses/$GIT_COMMIT' -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Content-Type: application/json' -X POST -d '{"state": "success", "context": "Jenkins", "description": "Jenkins pipeline succeeded", "target_url": "https://$CI_URL/job/${JOB_NAME}/$BUILD_NUMBER/console"}'
            // """
		}
        failure {
            echo "Ever CI/CD pipeline failed..."
			      // sh """
            //     curl 'https://api.github.com/repos/ever-co/${REPO_NAME}/statuses/$GIT_COMMIT' -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Content-Type: application/json' -X POST -d '{"state": "failure", "context": "Jenkins", "description": "Jenkins pipeline failed", "target_url": "https://$CI_URL/job/${JOB_NAME}/$BUILD_NUMBER/console"}'
            // """
        }
    }
}