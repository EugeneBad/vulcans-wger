stage ('Running tests'){
    parallel (
        "Gym, Config, and Email" : {
            node() {
                    ws('core-manager'){
                    venv()
                    sh '''
                    . venv/bin/activate
                    python3.6 ./manage.py test wger.core wger.manager --settings=wger.settings'''

                }
            }
        },
        "Nutrition and Utils": {
            node (){
                    ws('nutrition-utils'){
                    venv()
                    sh '''
                    . venv/bin/activate
                    python3.6 ./manage.py test wger.gym wger.utils wger.nutrition --settings=wger.settings'''

                }
            }
        },
        "Weight and Exercises": {
            node() {
                    ws('weight-exercises'){
                    venv()
                    sh '''
                    . venv/bin/activate
                    python3.6 ./manage.py test wger.weight wger.exercises --settings=wger.settings'''

                }
            }
        },
        "Core and Manager": {
            node (){
                    ws('core-manager'){
                    venv()
                    sh '''
                    . venv/bin/activate
                    python3.6 ./manage.py test wger.core wger.manager --settings=wger.settings'''

                }
            }
        }
    )
}

def venv(){
    stage ('Checkout'){
        //git branch: 'develop', url: 'https://github.com/EugeneBad/vulcans-wger.git'
        checkout scm
    }

    stage("Install dependencies"){
       sh '''
        python3.6 -m venv venv
        . venv/bin/activate
        pip install -r requirements_devel.txt
        pip install wger'''
    }

    stage("Create settings"){
        sh '''
        . venv/bin/activate
        wger create_settings --settings-path `pwd`/wger/settings.py --database-path `pwd`/database.sqlite
        invoke bootstrap_wger --settings-path `pwd`/wger/settings.py --no-start-server'''
    }
}
