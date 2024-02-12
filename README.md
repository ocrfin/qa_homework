# holistiplan

Holistiplan!

[![Built with Cookiecutter Django](https://img.shields.io/badge/built%20with-Cookiecutter%20Django-ff69b4.svg?logo=cookiecutter)](https://github.com/cookiecutter/cookiecutter-django/)
[![Black code style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)

## Settings

Moved to [settings](http://cookiecutter-django.readthedocs.io/en/latest/settings.html).

## Basic Commands
### Running the Vite Dev Server

This app integrates with a Vue frontend located in `vue_frontend`.
##### With Docker
The Vite dev server will automatically run in docker when started with the local.yml configuration.
```sh
docker-compose -f local.yml up
```



##### From the console
Alternatively you, may run the Vite dev server directly from the project directory:
```sh
cd vue_frontend
npm install
npm run dev
````

For more information, refer to the [Vue3 Vite Django Cookiecutter project](https://github.com/ilikerobots/cookiecutter-vue-django).



### Setting Up Your Users

- To create a **normal user account**, just go to Sign Up and fill out the form. Once you submit it, you'll see a "Verify Your E-mail Address" page. Go to your console to see a simulated email verification message. Copy the link into your browser. Now the user's email should be verified and ready to go.

- To create a **superuser account**, use this command:

      $ python manage.py createsuperuser

For convenience, you can keep your normal user logged in on Chrome and your superuser logged in on Firefox (or similar), so that you can see how the site behaves for both kinds of users.

### Type checks

Running type checks with mypy:

    $ mypy holistiplan

### Test coverage

To run the tests, check your test coverage, and generate an HTML coverage report:

    $ coverage run -m pytest
    $ coverage html
    $ open htmlcov/index.html

#### Running tests with pytest

    $ pytest

### Live reloading and Sass CSS compilation

Moved to [Live reloading and SASS compilation](https://cookiecutter-django.readthedocs.io/en/latest/developing-locally.html#sass-compilation-live-reloading).

### Celery

This app comes with Celery.

To run a celery worker:

```bash
cd holistiplan
celery -A config.celery_app worker -l info
```

Please note: For Celery's import magic to work, it is important _where_ the celery commands are run. If you are in the same folder with _manage.py_, you should be right.

To run [periodic tasks](https://docs.celeryq.dev/en/stable/userguide/periodic-tasks.html), you'll need to start the celery beat scheduler service. You can start it as a standalone process:

```bash
cd holistiplan
celery -A config.celery_app beat
```

or you can embed the beat service inside a worker with the `-B` option (not recommended for production use):

```bash
cd holistiplan
celery -A config.celery_app worker -B -l info
```

## Deployment

The following details how to deploy this application.

### Vue

For production deployment, the Vue frontend must be built into static resources, which will be served
using the same Django staticfiles strategy as the rest of your site.  

If you are using the production Docker configuration, this will be performed automatically when the images are built.  

Otherwise, you must build the static assets yourself as part of your build and deploy process, sometime before the 
`collectstatic` management command is run. The static assets may be built by running `npm run build` from within the 
`vue_frontend` directory. The resulting files will be placed into the `holistiplan/static/vue` directory 
and are handled subsequently as standard static assets. 

Note the setting `VUE_FRONTEND_USE_DEV_SERVER` dictates whether your Django app will be expecting to serve Vue assets 
from the Vite Dev Server or from a static build.  This setting defaults to the same as `DEBUG`, but can be modified as 
needed.
If you wish to build static Vue assets on the local Docker configuration, you may run:
`docker-compose -f local.yml run vite vite build`

For more information, refer to the [Vue3 Vite Django Cookiecutter project](https://github.com/ilikerobots/cookiecutter-vue-django).

### Docker

See detailed [cookiecutter-django Docker documentation](http://cookiecutter-django.readthedocs.io/en/latest/deployment-with-docker.html).

### Custom Bootstrap Compilation

The generated CSS is set up with automatic Bootstrap recompilation with variables of your choice.
Bootstrap v5 is installed using npm and customised by tweaking your variables in `static/sass/custom_bootstrap_vars`.

You can find a list of available variables [in the bootstrap source](https://github.com/twbs/bootstrap/blob/v5.1.3/scss/_variables.scss), or get explanations on them in the [Bootstrap docs](https://getbootstrap.com/docs/5.1/customize/sass/).

Bootstrap's javascript as well as its dependencies are concatenated into a single file: `static/js/vendors.js`.