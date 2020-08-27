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

    pip을 이용해 필요한 파일을 설치하세요:
    ```bash
    > pip3 install -r requirements.txt
    ```
*   *Anaconda users*

    TechnologyScout.net, 설명해주셔서 감사합니다:
    ```bash
    > while read requirement; do conda install --yes $requirement; done < requirements.txt    
    ```
    
## Database setup


Django는 박스 외부의 Sqllite3를 이용해 작동하도록 셋업되어 있으며, 이것 만으로도 모든 것을 실행함에 있어서 충분합니다. 하지만, 일부는 PostGreSQL를 사용함으로 상당히 빨라질겁니다. 
* Postgres를 설치하고 싶다면, 데이터베이스를 생성하기 전에 Postgres 설치 안내를 따라가세요.
* Postgres를 설치하고싶지 않다면, *Create and populate the MovieGEEKS databases* 섹션으로 넘어가세요. 

### [PostGreSQL-OPTIONAL] Install and use PostGreSQL

장고는 외부 데이터베이스 없이 웹사이트를 구동할 수 있도록 데이터베이스가 내장되어있습니다. 하지만, 다른 데이터베이스를 사용하면 더 빨라집니다. 개인적으로 PostGreSQL db를 잘 사용한 경험이 있습니다. 

####  Install and run PostGreSQL

먼저, Postgres를 설치하고 실행합니다. 
[여기에서](https://www.postgresql.org/download/) 여러분의 운영체제에 맞는 postgresql 버전을 다운로드 받으시고, 다운로드 페이지에 있는 설치 안내를 따라간 후에 실행해보세요. 

#### Create the database for MovieGEEK 

PostGreSQL의 어드민 툴 pgadmin을 이용해 데이터베이스를 만듭니다. 데이터베이스 이름은 `moviegeek`으로 해주세요. username과 password는 데이터베이스를 만들 때 사용한 것으로 넣어주세요. 앞으로 두 단계 더 진행한 후에 Django 세팅을 바꿔줄 때도 이 정보를 다시 사용할 겁니다.

#### Install the Python database driver 

PostGreSQL 데이터베이스가 돌아간다면, 이제 Django가 데이터베이스와 이야기할 수 있게 해주는 파이썬 드라이버를 준비할 차례입니다. [Psycopg](https://www.psycopg.org/)를 사용하길 추천합니다. 
[여기](https://pypi.org/project/psycopg2/)에서 다운받으세요. 설치는 이 [안내](https://www.psycopg.org/docs/install.html)를 따라주세요.

####  Configure the Django database connection to connect to PostGreSql

PostGreSQL(혹은 다른 db)를 사용한다면, MovieGEEKS에 사용할 장고 데이터베이스 연결을 설정해야 합니다. 아래 각 단계를 따라주세요. 자세한 내용이 필요하다면, 장고 공식문서를 참고하세요. 

`prs_project/settings.py`를 열어주세요. 

아래 내용을 업데이트해주세요:

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
USER, PASSWORD, HOST, PORT 필드를 업데이트해주어야 합니다 :
* USER(db_user): MovieGEEK 데이터베이스를 생성할 때 사용한 유저 네임을 이용해주세요. 
* PASSWORD(db_user_password): MovieGEEK 데이터베이스를 생성할 때 사용한 패스워드를 이용해주세요. 
* HOST (db_host): localhost (개인 머신에 설치했다면)
* PORT (db_port_number): 5432 (디폴트 포트 넘버)

자세한 정보가 필요하다면 이 문서를 참고하세요.[link](https://docs.djangoproject.com/en/2.2/ref/databases/)

### Create and populate the MovieGEEKS databases

Postgresql 사용여부와 관계없이 독자분들은 모두 아래 단계를 따라주셔야합니다. 

#### Create the MovieGEEKS databases

데이터베이스 연결이 설정되었다면, 아래 커맨드를 이용해 장고와 이 웹사이트를 구동하는데 필요한 데이터베이스를 만들 수 있을 겁니다. 

```bash
> python3 manage.py makemigrations
> python3 manage.py migrate --run-syncdb
```

#### Populate the database 
MovieGEEKS 웹사이트에 필요한 데이터셋을 다운로드 받기 위해 아래 스크립트를 실행해주세요. 

경고: 파이썬 3.7 이상 사용하시는 맥 유저는 데이터베이스를 채워넣기전에, 아래 커맨드를 실행해주셔야합니다. 
`/Applications/Python\ 3.7/Install\ Certificates.command`. 자세한 사항은 [여기](https://bugs.python.org/issue28150)와 [여기](https://timonweb.com/tutorials/fixing-certificate_verify_failed-error-when-trying-requests_html-out-on-mac/)를 참고해주세요.

데이터베이스를 채워주기 위해 아래 커맨드를 실행해주세요. 

```bash
> python3 populate_moviegeek.py
> python3 populate_ratings.py
```
경고: 시간이 꽤 걸릴 수 있습니다. 

###  Create an ID for themoviedb.org

themoviedb.org의 이미지를 사용하기 위해서는 아이디를 만들어야합니다. 

* [https://www.themoviedb.org/account/signup](https://www.themoviedb.org/account/signup)에 접속합니다.
* 가입해주세요.
* 로그인 한 다음에, account settings에서 [create an API](https://www.themoviedb.org/settings/api)로 들어가주세요. 설정화면은 우측 상단에 있는 아바타를 클릭하시면 됩니다().
 
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
