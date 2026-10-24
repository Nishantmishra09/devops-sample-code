pipeline {
    agent any

    environment {
        DEPLOY_DIR = "${WORKSPACE}/python-app-deploy"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                    python3 --version
                    python3 -m venv .venv
                    . .venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                    . .venv/bin/activate
                    python -m unittest discover -s . -p "test_*.py" -v
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to the mock deployment directory...'
                sh '''
                    mkdir -p "$DEPLOY_DIR"
                    cp app.py requirements.txt test_app.py "$DEPLOY_DIR/"
                    cp -r .venv "$DEPLOY_DIR/.venv"
                    echo "Deployment files:"
                    ls -la "$DEPLOY_DIR"
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Starting Flask application in the background...'
                sh '''
                    cd "$DEPLOY_DIR"
                    if [ -f app.pid ] && kill -0 "$(cat app.pid)" 2>/dev/null; then
                        kill "$(cat app.pid)" || true
                    fi
                    nohup .venv/bin/python app.py > app.log 2>&1 &
                    echo $! > app.pid
                    sleep 2
                    if ! kill -0 "$(cat app.pid)" 2>/dev/null; then
                        cat app.log
                        exit 1
                    fi
                    cat app.log
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Checking the running application over HTTP...'
                sh '''
                    .venv/bin/python - <<'PY'
import time
import urllib.request

url = "http://127.0.0.1:5000/"
last_error = None
for _ in range(10):
    try:
        with urllib.request.urlopen(url, timeout=2) as response:
            body = response.read().decode("utf-8")
            print("HTTP status:", response.status)
            print("Response body:", body)
            assert response.status == 200
            assert body == "Hello, Jenkins Multi-Stage Pipeline!"
            break
    except Exception as exc:
        last_error = exc
        time.sleep(1)
else:
    raise SystemExit(f"Application health check failed: {last_error}")
PY
                '''
            }
        }
    }

    post {
        always {
            sh '''
                if [ -f "$DEPLOY_DIR/app.pid" ]; then
                    kill "$(cat "$DEPLOY_DIR/app.pid")" 2>/dev/null || true
                fi
            '''
            archiveArtifacts artifacts: 'python-app-deploy/app.log', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the console output and archived app.log.'
        }
    }
}
