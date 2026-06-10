pipeline {
    agent any
    
    environment {
        PATH = "/opt/homebrew/bin:/opt/homebrew/sbin:$PATH"
    }
    
    stages {
        stage('Install Apache (httpd)') {
            steps {
                echo "Встановлюємо Apache через Homebrew..."
                sh '''
                brew install httpd
                
                sed -i '' 's/Listen 8080/Listen 8081/g' /opt/homebrew/etc/httpd/httpd.conf
                
                brew services restart httpd
                
                sleep 5
                '''
            }
        }
        
        stage('Generate Fake Requests') {
            steps {
                echo "Генеруємо фейкові запити на порт 8081..."
                sh 'curl -s http://localhost:8081/this-page-does-not-exist || true'
                sh 'curl -s http://localhost:8081/another-fake-request || true'
            }
        }
        
        stage('Check Apache Logs (4xx & 5xx)') {
            steps {
                echo "Шукаємо 4xx та 5xx помилки в access_log..."
                sh '''
                grep -E \'\" (4[0-9]{2}|5[0-9]{2}) \' /opt/homebrew/var/log/httpd/access_log || true
                '''
                
                echo "Перевіряємо останні записи з error_log:"
                sh 'tail -n 10 /opt/homebrew/var/log/httpd/error_log || true'
            }
        }
    }
}
