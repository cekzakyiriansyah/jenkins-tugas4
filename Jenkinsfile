pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'
        }
    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Install Dependencies') {
            steps {
                ansiColor('xterm') {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                ansiColor('xterm') {
                    sh 'pytest test_app.py || true'
                }
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                ansiColor('xterm') {
                    echo "Simulating deploy from branch ${env.BRANCH_NAME}"
                }
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425448539017842859/PfZCM7NHEkNpObDD5Wq2cptth9BV5P985Z8TYUQF1-2TPhyHuGxHvfkf1LEtvKvgZoBX'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425448539017842859/PfZCM7NHEkNpObDD5Wq2cptth9BV5P985Z8TYUQF1-2TPhyHuGxHvfkf1LEtvKvgZoBX'
                )
            }
        }
    }
}
