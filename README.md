# The MovieGEEK 설치 가이드

MovieGEEK는 Kim Falk의 책 Practical Recommender Systems와 함께 구현 된 웹 사이트입니다.
추천 시스템의 작동 방식과 구현 방법을 보여주기 위해 이 책에서 사용됩니다.
이 책은 알고리즘의 작동 방식을 설명하고 사이트 작동 방식에 대한 자세한 정보를 제공합니다.

이 웹사이트는 실제 여러분만의 콘텐츠 사이트를 제작 및 운영하는 데 사용할 수 있는 튜토리얼이나 플러그 앤 플레이 웹사이트로 만들어진 것이 아닙니다. 

## Thanks!
이 사이트는 [MovieTweetings](https://github.com/sidooms/MovieTweetings) 가 아니었으면 존재하기 어려웠을겁니다. 
사이트에 사용된 데이터셋과 이미지는 [themoviedb.org](https://www.themoviedb.org) API에서 제공받았습니다. 
그들의 모든 작업에 대해 그들 모두에게 큰 감사를 전하고 싶습니다.

## Project Setup

### Install Python 3.x
 
The MovieGEEK 웹사이트를 운영하려면 Python 3.x 가 설치되어있어야 합니다. Practical Recommender Systems가 파이썬을 알려주진 않습니다. 여러분은 파이썬을 읽을 줄 알아야하며, 프로그래밍 경험이 있다면 이 웹사이트를 구현하는 작업이 보다 쉬울 것입니다. 

[Hitchhikers guide to Python](http://docs.python-guide.org/en/latest/)는 “파이썬 초보 개발자나 전문 개발자가 실제 업무에서 일반적으로 사용하는 파이썬 설치, 컨피규레이션, 사용방법의 모범사례 핸드북"입니다. 맥이나 리눅스 유저는 이 가이드에 있는 안내를 따라가세요. 

윈도우 유저라면, 파이썬이나 파이썬 패키지를 사용하는 것이 다소 까다로울 수 있으므로, 가장 간단하게 설치할 수 있는 [아나콘다](https://www.anaconda.com/distribution/) 패키지를 이용하길 추천합니다. 
원한다면 the Hitchhiker’s Guide 의 윈도우 버전 안내를 활용할 수도 있겠지만, 저는 항상 아나콘다 패키지를 이용해왔습니다. 

## Download source code
소스코드를 다운 받는 방법은 두 가지 입니다 - 소스코드 zip 파일을 다운받거나 Git을 사용하시면 됩니다. 

* *Zip 파일 다운받기*
 
   [깃허브의 MovieGEEK](https://github.com/practical-recommender-systems/moviegeek)의 메인 디렉토리에서, 
   초록색 "클론 또는 다운로드" 버튼을 클릭하시고 컴퓨터에 zip 파일을 다운받으세요. 
   
* *Git 사용하기*

   이 리파지토리를 바로 클론하거나 당신의 깃허브에 포크한 다음에 클론하세요. 아래 코맨드를 사용하면 컴퓨터에 복사본이 만들어질 겁니다. 
   `> git clone https://github.com/practical-recommender-systems/moviegeek.git`

## Create a virtual environment for the project

코드 실행 전에, 가상 환경을 만들어야 합니다. 추가 정보가 필요하다면, The Hitchhiker’s Guide를 [참고하세요](https://docs.python-guide.org/dev/virtualenvs).
virtualenv이 설치되었는지 확인하고, 설치되어 있지 않다면 [이 내용](https://docs.python-guide.org/dev/virtualenvs/#lower-level-virtualenv)을 읽어보세요.
아나콘다를 사용하거나 the Hitchhiker’s Guide를 따라해보면, 설치가 되어있을겁니다. 설치가 되어있는지 확인하기 위해서는 아래 커맨드를 사용하세요. 

```bash
> virtualenv --version
```
virtualenv가 설치되어있다는 걸 확인하셨다면, 아래 커맨드를 이용해 가상 환경을 만들어주세요. (아나콘다 유저분들은 아나콘다 커맨드를 이용해주세요)

*   *Non-Anaconda users*:
    ```bash
    > cd moviegeek
    > virtualenv -p python3 prs
    > source prs/bin/activate
    ```
*   *Anaconda users*:
    ```bash
    > cd moviegeek
    > conda create -n prs python=3.6
    > conda activate prs
    ```
    여기서 3.6은 여러분이 이용하고 있는 버전 3.x로 고쳐서 사용하여샤 합니다. 

### Get the required packages
이부분에도 아나콘다는 따로 설명이 있으니, 주의하세요!

*   *Non-Anaconda users* 

    Use pip to install the required files:
    ```bash
    > pip3 install -r requirements.txt
    ```
*   *Anaconda users*

    Thanks, TechnologyScout.net for these instructions:
    ```bash
    > while read requirement; do conda install --yes $requirement; done < requirements.txt    
    ```
    
## Database setup

Django is setup to run with Sqllite3 out of the box, which is enough to run everything. However, some 
things will be considerably faster if you install PostGreSQL. 

*   If you do want to install Postgres, follow the Postgres installation steps before you create the databases. 
*   If you don’t want to install Postgres, jump to *Create and populate the MovieGEEKS databases* section.

### [PostGreSQL-OPTIONAL] Install and use PostGreSQL

Django comes with a database that enables you to run the website without an external database. However, using another 
database makes it faster. I had good experiences using the PostGreSQL db.
 
####  Install and run PostGreSQL
First, install Postgres and run it. 
Download the correct postgresql version for your operating system [here](https://www.postgresql.org/download/),
 and follow the instructions on from the download page to install and run it. 

#### Create the database for MovieGEEK 

Use PostGreSQL’s admin tool pgadmin to create a database. Name it `moviegeek`. Write down which username and password you usd to create the database. You will use that information in two steps from now when you change the Django settings.

#### Install the Python database driver 

Once the PostGreSQL database is spinning, it’s time for the Python driver, which enables Django to talk with the 
database. I recommend using [Psycopg](https://www.psycopg.org/). Download it [here](https://pypi.org/project/psycopg2/). 
Install it following these [instructions](https://www.psycopg.org/docs/install.html). 

####  Configure the Django database connection to connect to PostGreSql

If you use a PostGreSQL (or another db) you need to configure the Django database connection for MovieGEEKS, follow 
these steps. Refer to Django docs here if you need more details. 

Open `prs_project/settings.py` 

Update the following:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'moviegeek',                      
        'USER': 'db_user',
        'PASSWORD': 'db_user_password',
        'HOST': 'db_host',
        'PORT': 'db_port_number',
    }
}
```
Update the USER, PASSWORD, HOST, and PORT fields:
* USER(db_user): Use the user name you created with the MovieGEEK database
* PASSWORD(db_user_password): Use the password you created with the MovieGEEK database
* HOST (db_host): localhost (if you have have installed it on your private machine)
* PORT (db_port_number): 5432 (the default port)

For more information please refer to the Django documentation [link](https://docs.djangoproject.com/en/2.2/ref/databases/)

### Create and populate the MovieGEEKS databases
Everyone must follow these steps, whether or not you are using PostGreSQL.

#### Create the MovieGEEKS databases

When the database connection is configured, you can run the following commands to create the databases that Django 
and this website need to run.

```bash
> python3 manage.py makemigrations
> python3 manage.py migrate --run-syncdb
```

#### Populate the database 
Run the following script to download the datasets for the MovieGEEKS website. 

WARNING: Mac users running Python 3.7 or higher, before you populate the databases, you need to run this command. 
`/Applications/Python\ 3.7/Install\ Certificates.command`. More details [here](https://bugs.python.org/issue28150)
 and [here](https://timonweb.com/tutorials/fixing-certificate_verify_failed-error-when-trying-requests_html-out-on-mac/).

Everyone, run these commands to populate the databases.
```bash
> python3 populate_moviegeek.py
> python3 populate_ratings.py
```
WARNING: This might take some time.

###  Create an ID for themoviedb.org

You have to create an ID with themoviedb.org to use its pictures.

* Go to [https://www.themoviedb.org/account/signup](https://www.themoviedb.org/account/signup) 
* Sign up
* Login, go to your account settings and [create an API](https://www.themoviedb.org/settings/api). You can access 
settings by clicking the avatar in the upper right-hand corner (the default is a blue circle with a white logo in it). 
Then you’ll see settings on the left. 
* Create a file in the moviegeek directory called ".prs" 
* Open .prs and add { "themoviedb_apikey": <INSERT YOUR APIKEY HERE>}
Remember to remove the "<" and ">" When you are finished, the file contents should look something like 
{"themoviedb_apikey": "6d88c9a24b1bc9a60b374d3fe2cd92ac"}

### Start the web server
To start the development server, run this command:
```bash
> python3 manage.py runserver 127.0.0.1:8000
```
Running the server like this will make the website available [http://127.0.0.1:8000](http://127.0.0.1:8000) 

WARNING: Other applications also use this port so you might need to try out 8001 instead.

## Closing down
When you are finished running the project you can close it down doing the following steps, or simply close the 
terminal where the server is running. 

* Close down the server by pressing -c
* Exit the virtual environment

Non-Anaconda users
```bash
>  deactivate
```
Anaconda users
```bash
> conda deactivate
```

## Restart

To restart the project again do the following:

*   *Non-Anaconda users*:
    ```bash
    > cd moviegeek
    > source prs/bin/activate
    ```
*   *Anaconda users*:
    ```bash
    > cd moviegeek
    > conda activate prs
    ```
    
Start the web server again by running the following command:
```bash
> python3 manage.py runserver 127.0.0.1:8000
```
