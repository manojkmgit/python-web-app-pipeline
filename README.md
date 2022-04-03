# python-web-app
Python app for CI CD

## Clone the repo 
```sh
git clone https://github.com/singh-ashok25/python-web-app-test-cases.git
cd python-web-app
```

### To understand new tool pipenv, refer to this

https://pypi.org/project/pipenv/

## Setup virtual env and install requirements
```sh
sudo apt -y install python3.9
pip3 install --upgrade pip
pip3 install virtualenv
virtualenv -p /usr/bin/python3.9 venv
source venv/bin/activate
pip3 install -r requirements.txt
```

### Run test cases
```sh
pipenv shell
pytest --cov=tests --cov-report term -vs

```

### Start the app
```sh
python3 app/app.py 
curl http://127.0.0.1:5000/
```

