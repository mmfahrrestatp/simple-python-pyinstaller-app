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

    docker.image('cdrx/pyinstaller-linux:python2').inside {
        try {
            stage('deploy') {
                sh 'pyinstaller --onefile sources/add2vals.py' 
            } 
        }   finally {
                archiveArtifacts 'dist/add2vals'
        }
    }
}