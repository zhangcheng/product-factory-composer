# OpenUnited

## Git Setup and Forking Instructions

**Step 1**: Please ensure you have git and ssh setup, and that Github knows who you are via ssh through the magic of keys! To test this, run the following in the terminal:

ssh -T git@github.com

Github should give a response that includes your username. If this doesn't happen, then please follow [these instructions for connecting to github with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) or these general [git setup instructions](https://docs.github.com/en/get-started/quickstart/set-up-git). You may also need to add your SSH key to [your settings](https://github.com/settings/keys) in your Github account.

**Step 2**: in your web browser via github.com, fork all three of the repositories:

1. Composer: https://github.com/OpenUnited/product-factory-composer
2. Backend: https://github.com/OpenUnited/product-factory-backend
3. Frontend: https://github.com/OpenUnited/product-factory-frontend

New to forking? Here is some [general forking info](https://docs.github.com/en/get-started/quickstart/fork-a-repo)

**Step 3**: Clone your "Composer fork" (SSH recommended)

Clone your fork of the Composer repo to your development computer via SSH. This will mean running for example:

git clone git@github.com:*yourusername*/product-factory-composer.git

where "yourusername" is replaced with your github username :-)

**Step 4**: Change your composer fork to point to your forks of the frontend and backend repos

Edit the file *product-factory-composer/.gitmodules* locally, updating the "url" parameter of the backend and frontend to refer to your forks. This simply means changing "OpenUnited" to your github username.


**Step 5**: Continue with "How to run the project" as described below. 

Questions? Please [accept our invitation](https://discord.com/invite/T3xevYvWey) to join the [OpenUnited Discord Server](https://discord.com/invite/T3xevYvWey) and then visit the [#tech-setup-troubleshooting](https://discord.com/channels/910309960477446244/929010516666503208) channel and ask for assistance.

## How to run the project

1. Initialize project submodules
```
git submodule init
git submodule update
```
2. Create backend/.env file from backend/.env.template and update the file variables with your information.
```
cp backend/.env.template backend/.env
```

3. Run backend and frontend:
```sh
docker-compose up
```
> You need to have [docker engine](https://docs.docker.com/engine/install/) and [docker-compose](https://docs.docker.com/compose/install/) installed.

4. Import dummy data
If you want to set initial default data for testing please run this command to fill the database with some information.
```
docker-compose exec backend python manage.py dummy_data
```

5. Create a superuser
```sh
docker-compose exec backend python manage.py createsuperuser
```

6. Open the application [http://0.0.0.0:8080](http://0.0.0.0:8080)

7. The admin panel is available [http://0.0.0.0:8080/admin](http://0.0.0.0:8080/admin)

8. The application works in development mode (max 2 products & 5 users/persons).
The server requires two license files to be present before you can run it. You can find the developer license files in the `backend` repository. They are `developer.license` and `developer.license_key.pub`. You need to point to these two files in the `.env` file. Corresponding env variable names are `LICENSE_FILE` and `LICENSE_PUB_KEY`. After you make the changes, the last two lines of your `.env` file should be like this:
```
LICENSE_FILE=developer.license
LICENSE_PUB_KEY=developer.license_key.pub
```

If after starting the project in the console the error is: "Invalid HTTP_HOST header: 'localhost:8000'. You may need to add 'localhost' to ALLOWED_HOSTS", and frontend crashes after first render.
1. Create a local_settings.py under product-factory-composer/backend/backend folder.
2. Add the following codes.
```
ALLOWED_HOSTS = [
    "localhost",
    "localhost:3000"
]
```


### Frontend
The default application is in development mode, where fake user authorization is available in the system `/switch-test-user`.
To prevent this page from being accessed and authorizing using AM:
1. Create the `frontend/.env` file.
2. Define `APPLICATION_MODE=production` variable.


## Running tests

1. Backend tests:
```
docker-compose exec backend pytest
```

2. Frontend tests:
```
docker-compose exec frontend npm test
```

## Data Model

[data model diagram](https://github.com/OpenUnited/product-factory-composer/blob/master/docs/diagrams/openunited-data-model%20v1.0.png)

## Design Choices

A concept called "ProductTree" defines what a product does. The ProductTree comprises a set of nested Capabilities in a tree-like structure. ProductTree helps to ensure that people understand what a product does, which sounds obvious however to date frameworks have only focussed on the work to be done and not what the product does.

The work to be done on a product is organised separately to the ProductTree but in a related way.  The units of work are defined as Tasks, and these are grouped into Initiatives.

Whilst we deliberately separate what the product does (ProductTree) from the work (Initiatives/Tasks) to be done, these are related:

* Initiatives belong to a Capability in the ProductTree
* An Initiative's Tasks are by default related to the same Capability as that Initiative
* The Capability-Task relationship can be overriden, Tasks are allowed to relate to a different Capability than their Initiative if desired

### Capability
Capability is a way of breaking down the functional and non-functional areas of the product. The nested set of Capabilities related to a Product is referred to as the ProductMap.  An example ProductMap from Mailchimp:

* Audience Management
  * Marketing CRM
    * Audience dashboard
    * Import contacts
    * Export data
    * Segmentation tool
  * Signup Forms
  * Segmentation
  * Behavioral Targeting
  * Predicted Demographics
* Creative Tools
  * Content Studio
  * Creative Assistant (Beta)
  * Dynamic Content
  * Subject Line Helper
  * Campaign Templates
* Marketing Automation
  * Customer Journeys
  * Integration
  * Transactional Email
* Insight & Analytics
  * Reports
  * Smart Recommentations
  * A/B Testing
  * Serverys

### Task
Tasks are units of work to be done. Tasks are grouped into Initiatives and both Tasks and Initiatives relate to a capability in the ProductMap.

For example, a Task might be: As a user, I want to be able to tag contacts. Implement the ability for contacts to be tagged in the UI and backend.

### Initiative
Initiatives are a group of Tasks that relate to each other. Each initiative belongs to a Capability and can have multiple Tasks. The stories by default have their initiative's Category.

For example, an Initiative might be: Create new Admin Console
