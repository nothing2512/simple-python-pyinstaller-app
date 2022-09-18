node {
  stage('checkout') {
    checkout scm
  }
  
  stage('build') {
    docker.image('python:2-alpine').inside {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  
  stage('test') {
    docker.image('qnib/pytest').inside {
      try {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      } catch (e) {
      } finally {
        junit 'test-reports/results.xml'
      }
    }
  }
  
  stage('deliver') {
    docker.image('cdrx/pyinstaller-linux:python2').inside {
      try {
        sh 'pyinstaller --onefile sources/add2vals.py'
        archiveArtifacts 'dist/add2vals'
      } catch (e) {
      } finnally {
      }
    }
  }
}
