
node {
    
    stage('checkout') {
        git branch: 'develop', url: 'https://github.com/EugeneBad/vulcans-wger.git'
    }
    
    stage('Install Depenedencies'){
        sh '''
            python3 -m venv venv
            . venv/bin/activate
            pip install -r requirements_devel.txt
            pip install wger
        '''
    }
  
    stage ('Bootstrap Application'){
        sh '''
            . venv/bin/activate
            wger create_settings --settings-path `pwd`/wger/settings.py --database-path `pwd`/database.sqlite
            invoke bootstrap_wger --settings-path `pwd`/wger/settings.py --no-start-server
        '''    
    }
    
    stage ('Run tests') {
        sh '''
            . venv/bin/activate
            python3 ./manage.py test --settings=wger.settings
        '''    
    }
    
}
