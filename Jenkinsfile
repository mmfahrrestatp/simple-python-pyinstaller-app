node {  
    docker.image('python:2-alpine').inside {
        stage('Build') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
        }
    }
    docker.image('qnib/pytest').inside {
        try {
            stage('Test') {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py' 
            } 
        }   finally {
                junit 'test-reports/results.xml'
        }
    }

    docker.image('python:3.9').inside('-u root') {
        stage('Manual Approval') {
            input message: "Lanjutkan ke tahap Deploy?"
        }
        try {
            stage('Deploy') {
                sh 'pip install pyinstaller'
                sh 'pyinstaller --onefile sources/add2vals.py'
                sleep time: 1, unit: 'MINUTES'
                echo 'one minute passed'
            } 
        }   finally {
                archiveArtifacts 'dist/add2vals'
        }
    }
}