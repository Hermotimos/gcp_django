# STEPS:
# rename everywhere 'mysite' to the desired project name
# prepare env (requires database setup)
# create & activate virtualenv
# pip install -r requirements.txt (!! remove stuff for Google Storage if not needed together with mysite.storages.py)

# python manage.py migrate
# python manage.py createsuperuser
# python manage.py makemigrations polls
# python manage.py migrate
# python manage.py collectstatic (!! uncomment the right static conf in settings.py)
# python manage.py runserver
