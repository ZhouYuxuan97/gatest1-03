pipeline { 
    // agent any 
    agent {            
        docker {
            image 'ubuntu-with-git'
            args '-u root'
        }
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh '''
                    apt-get update
                    apt-get install -y build-essential
                '''
            }
        }

        stage('Download repository (single branch)') {
            steps {
                sh '''
                    rm -rf sqlite
                    git clone --single-branch --branch master https://github.com/sqlite/sqlite.git
                    cd sqlite
                    ./configure
                '''
            }
        }

        stage('Record compilation time') {
            steps {
                sh '''
                    START_TIME=$(date +%s)
                    cd sqlite
                    make
                    END_TIME=$(date +%s)
                    COMPILATION_TIME=$((END_TIME - START_TIME))
                    echo "Compilation time: $COMPILATION_TIME seconds"
                '''
            }
        }

        stage('Verify Compilation') {
            steps {
                sh '''
                    cd sqlite
                    if [ -f ./sqlite3 ]; then
                        echo "SQLite compiled successfully."
                        ./sqlite3 --version
                    else
                        echo "SQLite compilation failed."
                    fi
                '''
            }
        }
    }
}
