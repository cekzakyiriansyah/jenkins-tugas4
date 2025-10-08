stages {
    stage('Install Dependencies') {
        steps {
            sh 'pip install -r requirements.txt'
        }
    }

    stage('Run Tests') {
        steps {
            sh 'pytest test_app.py'
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
            echo "Simulating deploy from branch ${env.BRANCH_NAME}"
        }
    }
}

post {
    success {
        script {
            def payload = [content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"]
            httpRequest(
                httpMode: 'POST',
                contentType: 'APPLICATION_JSON',
                requestBody: groovy.json.JsonOutput.toJson(payload),
                url: 'https://discord.com/api/webhooks/1425457053001842809/JcIM7HeB3LmRAM0kZ3xaoe8x56kvAybSo1L5V6Y9hSLgdp6t2mGPqYgbPAKnYbvCj2Kq'
            )
        }
    }

    failure {
        script {
            def payload = [content: "❌ Build FAILED on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"]
            httpRequest(
                httpMode: 'POST',
                contentType: 'APPLICATION_JSON',
                requestBody: groovy.json.JsonOutput.toJson(payload),
                url: 'https://discord.com/api/webhooks/1425457053001842809/JcIM7HeB3LmRAM0kZ3xaoe8x56kvAybSo1L5V6Y9hSLgdp6t2mGPqYgbPAKnYbvCj2Kq'
            )
        }
    }
}
