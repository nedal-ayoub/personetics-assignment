pipeline {

    agent { label "master" }

    options {
        ansiColor('xterm')
        disableConcurrentBuilds()
    }

    stages {
        // Stage conf to setup environment variable for build
        stage("conf") {
            steps {
                script {
                    def pom = readMavenPom file: 'pom.xml'
                    env['PROJ'] = pom.artifactId
                    def inventory = readTrusted 'deployment/inventory'
                    env['SERVER_IP'] = inventory.split('\n')[1]
                }
            }
        }
        // Building the artifacts
        stage("build") {
            agent {
                docker {
                    image "maven:3-jdk-8-slim"
                    args "-v \$HOME/.m2:/root/.m2"
                }
            }
            steps {
                sh 'mvn --version'
                sh 'mvn clean install -ntp -B -e'
                stash includes: 'target/demo.jar', name: 'demo-jar'
            }
        }

        // building the image locally
        stage("build img") {
            steps {
                unstash 'demo-jar'
                script {
                    env["IMAGE"] = "${env.PROJ}:${env.BRANCH_NAME}.${env.BUILD_ID}"
                    def customImage = docker.build(env["IMAGE"])
                }
            }
        }

        // saving the image for deployment (we don't have docker registry)
        stage("prepare image to deploy") {
            // This step will only run when merging to release branch
            // TODO 2 create branch release* in VCS and do Pull Request
            when {
                branch "release*"
            }
            // Adding agent to make sure we'll use  the same (@2) workspace
            agent { label "master" }
            // TODO 3 add a command to save docker image as tar file in the deployment folder
            steps {
                sh "[write the cmd here]"
            }
        }

        // deploy the app (image)
        stage("deploy") {
            // This step will only run when merging to release branch
            // TODO 2 create branch release* in VCS and do Pull Request
            when {
                branch "release*"
            }
            agent {
                dockerfile {
                    dir 'deployment'
                    args '-v /home/ec2-user/.ansible:/home/centos/.ansible'
                }
            }
            steps {
                sh 'ls -la'
                sh 'ls -la deployment'
                ansiblePlaybook(
                        colorized: true,
                        // TODO 4 On Jenkins (http://jenkins_url:8080/credentials/) create a credentials secret (SSH username with private key) with the provided key
                        credentialsId: '[credentials-id]',
                        disableHostKeyChecking: true,
                        // TODO 4 add new image param with image name as value i.e. image=[?]
                        extras: "-e server_ip=${env.SERVER_IP} " +
                                "-e project_name=${env.PROJ} " +
                                "-vvv",
                        // TODO 4 add ip to inventory file
                        inventory: 'deployment/inventory',
                        playbook: 'deployment/deploy.yml'
                )
            }
        }

        // Test app≠
        stage("test-deployment") {
            // This step will only run when merging to release branch
            // TODO 2 create branch release* in VCS and do Pull Request
            when {
                branch "release*"
            }
            steps {
                script {
                    sleep 30
                    sh "curl http://${env.SERVER_IP}:8080/greeting?name=katsok"
                }
            }
        }
    }
    post {
        always {
            script {
                def msg = """
${env.BUILD_TAG} - ${currentBuild.currentResult}
Build: ${currentBuild.displayName}
Result: ${currentBuild.currentResult}
"""
                writeFile encoding: 'utf-8', file: "job-number-${env.BUILD_NUMBER}", text: msg
            }
        }
    }
}
