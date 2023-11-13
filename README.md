# 2023-assignment4-eBayLite-solution from A490 Web Dev Course @ UAA by chezmarcbrown

credit: https://youtu.be/hJmkECmXyxY?si=ldRxjHGLyVZM631L
# Integrating Google Cloud Storage with Django using django-storages[google]

This guide will walk you through the process of setting up `django-storages[google]` to work with Google Cloud Storage in your Django application.

## Prerequisites

- A Google Cloud Platform account
- A project created in the Google Cloud Console
- Billing enabled for your project
- Google Cloud Storage bucket created

## Steps

### 1. Install django-storages and google-cloud-storage

```bash
pip install django-storages[google]
```

This command will install both `django-storages` and the necessary dependencies for Google Cloud Storage.

### 2. Set up Google Cloud Storage Bucket

Create a bucket in Google Cloud Storage if you haven't already. Note down the bucket name as you will need it for the Django settings.

### 3. Create a Service Account

In the Google Cloud Console:
- Go to the "IAM & Admin" section.
- Create a new service account.
- Assign it the "Storage Object Admin" role for your bucket.
- Create a key for this service account and download the JSON file.

### 4. Configure Django Settings

In your Django `settings.py` file, add or update the following configurations:

```python
from dotenv import load_dotenv
from google.cloud import storage

# Add 'storages' to your INSTALLED_APPS
INSTALLED_APPS = [
    # ...
    'storages',
    # ...
]
# Load environment variables from .env file
load_dotenv()

# Google Cloud Storage settings
GS_BUCKET_NAME = 'your-bucket-name'
GS_PROJECT_ID = 'your-google-cloud-project-id'

# Tell Django to use GCS for file storage
DEFAULT_FILE_STORAGE = 'storages.backends.gcloud.GoogleCloudStorage'

# Your Google Cloud credentials JSON file path
GOOGLE_APPLICATION_CREDENTIALS = os.environ.get('GOOGLE_APPLICATION_CREDENTIALS')

# Static files (CSS, JavaScript, Images)
STATIC_URL = '/static/'

# If you're using GCS for static files
STATICFILES_STORAGE = 'storages.backends.gcloud.GoogleCloudStorage'

# Media files 
MEDIA_URL = 'https://storage.googleapis.com/{bucket_name}/'.format(bucket_name=GS_BUCKET_NAME)

```

Make sure to replace `'your-bucket-name'`, `'your-google-cloud-project-id'`, and `'path/to/your/google-credentials.json'` with your actual bucket name, project ID, and path to your service account JSON file.

### 5. Set Environment Variable for Credentials

```bash
pip install python-dotenv
```

Create .env file in root.

```bash
GOOGLE_APPLICATION_CREDENTIALS="path/to/your/google-credentials.json"
```

### 6. Run Migrations

After changing ImageField or FileField to DEFAULT_FILE_STORAGE (uploads)
or path to folder in bucket to grab images.

Run Django migrations to apply any changes to the models:

```bash
python manage.py migrate
```

### 7. Run App

```bash
python manage.py runserver
```


