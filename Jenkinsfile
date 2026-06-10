pipeline {
    agent any
    
    stages {
        stage('Install Apache (httpd)') {
            steps {
                sh '''
                brew install httpd
                
                sed -i '' 's/Listen 8080/Listen 8081/g' /opt/homebrew/etc/httpd/httpd.conf
                
                brew services restart httpd
                
                sleep 5
                '''
            }
        }
        
        stage('Generate  Requests') {
            steps {
                sh 'curl -s http://localhost:8081/this-page-does-not-exist || true'
                sh 'curl -s http://localhost:8081/another-fake-request || true'
            }
        }
        
        stage('Check Apache Logs (4xx & 5xx)') {
            steps {
                sh '''
                grep -E \'\" (4[0-9]{2}|5[0-9]{2}) \' /opt/homebrew/var/log/httpd/access_log || true
                '''
                
                sh 'tail -n 10 /opt/homebrew/var/log/httpd/error_log || true'
            }
        }
    }
}
