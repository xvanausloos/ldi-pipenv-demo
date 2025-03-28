LDI PIPENV demo 

Created on 28/03/25

Resource: https://realpython.com/pipenv-guide/

`pip install pipenv`

Once you’ve done that, you can effectively forget about pip since Pipenv essentially acts as a replacement. It also introduces two new files, the Pipfile (which is meant to replace requirements.txt) and the Pipfile.lock (which enables deterministic builds).

Pipenv uses pip and virtualenv under the hood but simplifies their usage with a single command line interface.

First, spawn a shell in a virtual environment to isolate the development of this app:

` pipenv shell`

Install dependencies:

`pipenv install flask==0.12.1`

`pipenv install numpy`

If you want to install something directly from a version control system (VCS), you can! You specify the locations similarly to how you’d do so with pip. For example, to install the requests library from version control, do the following:

`pipenv install -e git+https://github.com/requests/requests.git#egg=requests` 

Note the -e argument above to make the installation editable. Currently, this is required for Pipenv to do sub-dependency resolution.

Let’s say you also have some unit tests for this awesome application, and you want to use pytest for running them. You don’t need pytest in production so you can specify that this dependency is only for development with the --dev argument:

`pipenv install pytest --dev`

Okay, so let’s say you’ve got everything working in your local development environment and you’re ready to push it to production. To do that, you need to lock your environment so you can ensure you have the same one in production:

`pipenv lock`

This will create/update your Pipfile.lock, which you’ll never need to (and are never meant to) edit manually. You should always use the generated file.

Now, once you get your code and Pipfile.lock in your production environment, you should install the last successful environment recorded:

`pipenv install --ignore-pipfile`

This tells Pipenv to ignore the Pipfile for installation and use what’s in the Pipfile.lock. Given this Pipfile.lock, Pipenv will create the exact same environment you had when you ran pipenv lock, sub-dependencies and all.

The lock file enables deterministic builds by taking a snapshot of all the versions of packages in an environment (similar to the result of a pip freeze).

Now let’s say another developer wants to make some additions to your code. In this situation, they would get the code, including the Pipfile, and use this command:

`pipenv install --dev`

you no longer have to force exact versions you don’t truly need to ensure your development and production environments are the same. You also don’t need to stay on top of updating sub-dependencies you “don’t care about.” This workflow with Pipenv, combined with your excellent testing, fixes the issues of manually doing all your dependency management.


