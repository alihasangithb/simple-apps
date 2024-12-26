pipeline {
    agent { label 'server1-alihasan' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url:'https://github.com/alihasangithb/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
                sonar-scanner \
                    -Dsonar.projectKey=simple-apps- \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.14.21:9000 \
                    -Dsonar.login=sqp_18e300f62748df39e19bca3407f780bd2b6506c8
                '''
            }
        }
        
        stage('Change file.env') {
            steps {
                sh'''
                cd apps
                sed -i 's/localhost/db/g' .env
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}