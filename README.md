# STEPS:

<!-- LOCAL -->
# rename everywhere 'mysite' to the desired project name
# prepare env (requires database setup)
# python -m venv myvenv
# source myvenv\Scripts\activate OR source myvenv/bin/activate
# pip install -r requirements.txt (!! remove stuff for Google Storage if not needed together with mysite.storages.py)

# python manage.py migrate
# python manage.py createsuperuser
# python manage.py makemigrations APPNAME
# python manage.py migrate
# create 'static' and 'media' dir
# python manage.py collectstatic (!! uncomment the right static conf in settings.py)
# python manage.py runserver

<!-- GCP -->
# create GCP project

# create GCP instance or a database in an existing instance (and a user)
# enable Cloud SQL API for the right project: https://console.cloud.google.com/apis/library/sqladmin.googleapis.com
# enable Secrets API for the right project: https://console.cloud.google.com/apis/enableflow?apiid=secretmanager.googleapis.com

# enter cloud console and: "gcloud config set project PROJECTNAME"
# git clone your repo (!! to the right dir !!)
# enter Code Editor
# create .env in GCP Code Editor and populate it with correct data
# create and update .gcloudignore file in order to avoid "This deployment has too many files" ERROR: ignore the virtual environment dir created before
<!-- .env, .gitignore etx. are not shown in Code Editor: see them in Terminal with "ls -al" -->

# open new Terminal and cd into correct dir
# follow the steps from LOCAL (skip the first step as it should be done with git clone repo)
# in Cloud Shell cd to where manage.py resides and: "gcloud app deploy"

# generate service account key:
    # IAM & Admin -> Service Accounts -> choose Project -> click Email link -> click KEYS tab
    # Add key -> Create new key -> Select JSON as the Key type -> Create
    # https://cloud.google.com/iam/docs/creating-managing-service-account-keys
    # add file gcp-service-account-key.json with the content of the key

# [if needed] add CORS configuration: go to Cloud Console and create a cors.json file within project root with following content:
<!--

[
  {
    "origin": ["https://hyllemath2.lm.r.appspot.com"],
    "responseHeader": ["Content-Type"],
    "method": ["GET"],
    "maxAgeSeconds": 3600
  }
]

-->
# then set CORS and confirm:
    # gcloud storage buckets update gs://PROJECTID.appspot.com --cors-file=cors.json
    # gcloud storage buckets describe gs://PROJECTID.appspot.com --format="default(cors)"

# create 'static' and 'media' dirs in app default bucket

# migrate db to GCP: dump local db after migrations an restore it to GCP:
    # if connecting from project B to project's A db instance - in A's IAM Admin -> IAM -> Grant Access -> create principal of the sam name as B's service account and grant it with Cloud SQL Client role

# in project A IAM Admin -> IAM give following permissions to the service account: Secret Manager Secret Accessor, Cloud SQL Client
# in Security -> Secret Manager create the secret with the name ex. "django_settings" as in settings.py line : "settings_name = os.environ.get("SETTINGS_NAME", "django_settings")"
    give following value (but maybe it can be left empty? It seems useless now, works fine without other env variables defined in this secret)
<!--
SECRET_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-->


