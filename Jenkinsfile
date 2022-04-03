pipeline {
    environment {
     dev_app_server = "172.31.39.182"
     devapp_user = "ubuntu"
   }
    agent any
    stages {
        stage('Building') {
           steps {
                echo 'Starting build'
                sh '''
                    sudo apt -y install python3.9
                    sudo apt -y install python3-pip
                    pip3 install --upgrade pip
                    pip3 install virtualenv
                    virtualenv -p /usr/bin/python3.9 venv
                    . venv/bin/activate
                    pip3 install -r requirements.txt
                '''
                echo 'Building Success'
            }
        }
        stage('Linting and Testing') {
            steps {
                parallel(
                    a: {
                        echo 'Starting lint'
                        sh '''
                        . venv/bin/activate
                        pylint --output-format=parseable --fail-under=95 app/app.py --msg-template="{path}:{line}: [{msg_id}({symbol}), {obj}] {msg}" | tee pylint.log || echo "pylint exited with $?"
                        '''
                        echo "Linting Success"
                    },
                    b: {
                        echo 'Starting test'
                        sh '''
                        . venv/bin/activate
                        pytest --cov=tests --cov-report term -vs
                        '''
                        echo "Testing Success"
                    }
                )
                }
        }
        stage('Deploy') {
            steps {
                echo 'Starting deploy to the app server'
                sh '''
                . venv/bin/activate
                #sudo su - ${app_user}
                ssh  ${app_user}@${app_server} "mkdir -p /home/${app_user}/webapp"
                ssh ${app_user}@${app_server} "install -d -m 0755 -o ${app_user} -g ${app_user} /home/${app_user}/webapp"
                rm -rf venv
                scp -pr /var/lib/jenkins/workspace/Pipe1/*  ${app_user}@${app_server}:/home/${app_user}/webapp/
                ssh  ${app_user}@${app_server} "cd /home/${app_user}/webapp/; . /home/${app_user}/.bashrc; . ./deploy.sh"
                '''
                echo "Deployment Success"
            }
        }
    }
}
