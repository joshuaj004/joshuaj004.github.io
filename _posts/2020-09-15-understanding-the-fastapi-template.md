---
layout: post
title:  "Understanding the FastAPI Cookiecutter Template"
categories: FastAPI Cookiecutter
date: 2020-09-15 06:26:00
---

In continuing to use FastAPI and setting up the [Full Stack FastAPI and PostgreSQL project generator](https://github.com/tiangolo/full-stack-fastapi-postgresql), and honestly the output is a little bit intimidating. 

```
josh@josh ~/firstfastapi> tree
.
├── backend
│   ├── app
│   │   ├── alembic
│   │   │   ├── env.py
│   │   │   ├── __pycache__
│   │   │   │   └── env.cpython-37.pyc
│   │   │   ├── README
│   │   │   ├── script.py.mako
│   │   │   └── versions
│   │   │       ├── d4867f3a4c0a_first_revision.py
│   │   │       └── __pycache__
│   │   │           └── d4867f3a4c0a_first_revision.cpython-37.pyc
│   │   ├── alembic.ini
│   │   ├── app
│   │   │   ├── api
│   │   │   │   ├── api_v1
│   │   │   │   │   ├── api.py
│   │   │   │   │   ├── endpoints
│   │   │   │   │   │   ├── __init__.py
│   │   │   │   │   │   ├── items.py
│   │   │   │   │   │   ├── login.py
│   │   │   │   │   │   ├── __pycache__
│   │   │   │   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   │   │   │   ├── items.cpython-37.pyc
│   │   │   │   │   │   │   ├── login.cpython-37.pyc
│   │   │   │   │   │   │   ├── users.cpython-37.pyc
│   │   │   │   │   │   │   └── utils.cpython-37.pyc
│   │   │   │   │   │   ├── users.py
│   │   │   │   │   │   └── utils.py
│   │   │   │   │   ├── __init__.py
│   │   │   │   │   └── __pycache__
│   │   │   │   │       ├── api.cpython-37.pyc
│   │   │   │   │       └── __init__.cpython-37.pyc
│   │   │   │   ├── deps.py
│   │   │   │   ├── __init__.py
│   │   │   │   └── __pycache__
│   │   │   │       ├── deps.cpython-37.pyc
│   │   │   │       └── __init__.cpython-37.pyc
│   │   │   ├── backend_pre_start.py
│   │   │   ├── celeryworker_pre_start.py
│   │   │   ├── core
│   │   │   │   ├── celery_app.py
│   │   │   │   ├── config.py
│   │   │   │   ├── __init__.py
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── celery_app.cpython-37.pyc
│   │   │   │   │   ├── config.cpython-37.pyc
│   │   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   │   └── security.cpython-37.pyc
│   │   │   │   └── security.py
│   │   │   ├── crud
│   │   │   │   ├── base.py
│   │   │   │   ├── crud_item.py
│   │   │   │   ├── crud_user.py
│   │   │   │   ├── __init__.py
│   │   │   │   └── __pycache__
│   │   │   │       ├── base.cpython-37.pyc
│   │   │   │       ├── crud_item.cpython-37.pyc
│   │   │   │       ├── crud_user.cpython-37.pyc
│   │   │   │       └── __init__.cpython-37.pyc
│   │   │   ├── db
│   │   │   │   ├── base_class.py
│   │   │   │   ├── base.py
│   │   │   │   ├── init_db.py
│   │   │   │   ├── __init__.py
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── base_class.cpython-37.pyc
│   │   │   │   │   ├── base.cpython-37.pyc
│   │   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   │   ├── init_db.cpython-37.pyc
│   │   │   │   │   └── session.cpython-37.pyc
│   │   │   │   └── session.py
│   │   │   ├── email-templates
│   │   │   │   ├── build
│   │   │   │   │   ├── new_account.html
│   │   │   │   │   ├── reset_password.html
│   │   │   │   │   └── test_email.html
│   │   │   │   └── src
│   │   │   │       ├── new_account.mjml
│   │   │   │       ├── reset_password.mjml
│   │   │   │       └── test_email.mjml
│   │   │   ├── initial_data.py
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── models
│   │   │   │   ├── __init__.py
│   │   │   │   ├── item.py
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   │   ├── item.cpython-37.pyc
│   │   │   │   │   └── user.cpython-37.pyc
│   │   │   │   └── user.py
│   │   │   ├── __pycache__
│   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   ├── main.cpython-37.pyc
│   │   │   │   ├── utils.cpython-37.pyc
│   │   │   │   └── worker.cpython-37.pyc
│   │   │   ├── schemas
│   │   │   │   ├── __init__.py
│   │   │   │   ├── item.py
│   │   │   │   ├── msg.py
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── __init__.cpython-37.pyc
│   │   │   │   │   ├── item.cpython-37.pyc
│   │   │   │   │   ├── msg.cpython-37.pyc
│   │   │   │   │   ├── token.cpython-37.pyc
│   │   │   │   │   └── user.cpython-37.pyc
│   │   │   │   ├── token.py
│   │   │   │   └── user.py
│   │   │   ├── tests
│   │   │   │   ├── api
│   │   │   │   │   ├── api_v1
│   │   │   │   │   │   ├── __init__.py
│   │   │   │   │   │   ├── test_celery.py
│   │   │   │   │   │   ├── test_items.py
│   │   │   │   │   │   ├── test_login.py
│   │   │   │   │   │   └── test_users.py
│   │   │   │   │   └── __init__.py
│   │   │   │   ├── conftest.py
│   │   │   │   ├── crud
│   │   │   │   │   ├── __init__.py
│   │   │   │   │   ├── test_item.py
│   │   │   │   │   └── test_user.py
│   │   │   │   ├── __init__.py
│   │   │   │   └── utils
│   │   │   │       ├── __init__.py
│   │   │   │       ├── item.py
│   │   │   │       ├── user.py
│   │   │   │       └── utils.py
│   │   │   ├── tests_pre_start.py
│   │   │   ├── utils.py
│   │   │   └── worker.py
│   │   ├── mypy.ini
│   │   ├── prestart.sh
│   │   ├── pyproject.toml
│   │   ├── scripts
│   │   │   ├── format-imports.sh
│   │   │   ├── format.sh
│   │   │   ├── lint.sh
│   │   │   ├── test-cov-html.sh
│   │   │   └── test.sh
│   │   ├── tests-start.sh
│   │   └── worker-start.sh
│   ├── backend.dockerfile
│   └── celeryworker.dockerfile
├── cookiecutter-config-file.yml
├── docker-compose.override.yml
├── docker-compose.yml
├── frontend
│   ├── babel.config.js
│   ├── Dockerfile
│   ├── nginx-backend-not-found.conf
│   ├── package.json
│   ├── public
│   │   ├── favicon.ico
│   │   ├── img
│   │   │   └── icons
│   │   │       ├── android-chrome-192x192.png
│   │   │       ├── android-chrome-512x512.png
│   │   │       ├── apple-touch-icon-120x120.png
│   │   │       ├── apple-touch-icon-152x152.png
│   │   │       ├── apple-touch-icon-180x180.png
│   │   │       ├── apple-touch-icon-60x60.png
│   │   │       ├── apple-touch-icon-76x76.png
│   │   │       ├── apple-touch-icon.png
│   │   │       ├── favicon-16x16.png
│   │   │       ├── favicon-32x32.png
│   │   │       ├── msapplication-icon-144x144.png
│   │   │       ├── mstile-150x150.png
│   │   │       └── safari-pinned-tab.svg
│   │   ├── index.html
│   │   ├── manifest.json
│   │   └── robots.txt
│   ├── README.md
│   ├── src
│   │   ├── api.ts
│   │   ├── App.vue
│   │   ├── assets
│   │   │   └── logo.png
│   │   ├── component-hooks.ts
│   │   ├── components
│   │   │   ├── NotificationsManager.vue
│   │   │   ├── RouterComponent.vue
│   │   │   └── UploadButton.vue
│   │   ├── env.ts
│   │   ├── interfaces
│   │   │   └── index.ts
│   │   ├── main.ts
│   │   ├── plugins
│   │   │   ├── vee-validate.ts
│   │   │   └── vuetify.ts
│   │   ├── registerServiceWorker.ts
│   │   ├── router.ts
│   │   ├── shims-tsx.d.ts
│   │   ├── shims-vue.d.ts
│   │   ├── store
│   │   │   ├── admin
│   │   │   │   ├── actions.ts
│   │   │   │   ├── getters.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── mutations.ts
│   │   │   │   └── state.ts
│   │   │   ├── index.ts
│   │   │   ├── main
│   │   │   │   ├── actions.ts
│   │   │   │   ├── getters.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── mutations.ts
│   │   │   │   └── state.ts
│   │   │   └── state.ts
│   │   ├── utils.ts
│   │   └── views
│   │       ├── Login.vue
│   │       ├── main
│   │       │   ├── admin
│   │       │   │   ├── AdminUsers.vue
│   │       │   │   ├── Admin.vue
│   │       │   │   ├── CreateUser.vue
│   │       │   │   └── EditUser.vue
│   │       │   ├── Dashboard.vue
│   │       │   ├── Main.vue
│   │       │   ├── profile
│   │       │   │   ├── UserProfileEditPassword.vue
│   │       │   │   ├── UserProfileEdit.vue
│   │       │   │   └── UserProfile.vue
│   │       │   └── Start.vue
│   │       ├── PasswordRecovery.vue
│   │       └── ResetPassword.vue
│   ├── tests
│   │   └── unit
│   │       └── upload-button.spec.ts
│   ├── tsconfig.json
│   ├── tslint.json
│   └── vue.config.js
├── README.md
└── scripts
    ├── build-push.sh
    ├── build.sh
    ├── deploy.sh
    ├── test-local.sh
    └── test.sh

52 directories, 189 files
```

Yupppp it's almost 250 *things* and I'm supposed to know what each one of them does. Okay, but really it's not that bad if we look a bit closer. 

For reference, here are the different sections and the number of files + dirs in each:

| Section                                         |Approx. # of files + dirs|
|-------------------------------------------------|-----|
| Backend                                         | 145 |
| Frontend                                        | 86  |
| Rest (Including dockerfiles, ymls, and scripts) | 10  |



# Backend Fluff 

For starters, any of the pycache dirs and their contents can be ignored. This accounts for almost 50 items (11 dirs, 36 files) so right off the bat almost 20% of this whole project is just noise and it would be nice to hide. I'm using VSCode, so this [stack overflow](https://stackoverflow.com/questions/30140112/how-do-i-hide-certain-files-from-the-sidebar-in-visual-studio-code) answer is very helpful. It was as simple as adding `**/__pycache__/**` to the files to exclude.

And then we have 14 `__init__.py`'s which are empty or handle a couple imports, either way not so intimidating.

In our backend/app/app/email-templates folder, theres a few basic email templates, nothing crazy, accounting for 3 dirs and 6 files.

Then there's various scripts and config files which handle stuff like test coverage and formatting. 

Approx noise: 81/145

# Frontend Fluff

18 images, icons, and favicon.
6 json and js config files.
3 plugins files.  

Honestly, I've never used Vue before so I'm certain there are some files that I'll late be able to identify as fluff, but right now we've got over 30% of the frontend files so good enough for me.

Approx noise: 27/86

# Rest
There's only like 10 files look over them yourself :P

I haven't even looked at the meat of this project yet, but I feel a lot better already. Once I have a good grasp on what is going on, I'll probably put out a Part II of this for the backend and a Part III for the frontend.