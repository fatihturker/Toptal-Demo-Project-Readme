# Toptal *Confidential Project Name*
*Confidential Project Name* demo project assigned by Toptal.
### Features
  * Confidential.

### Tech Stack
Used number of open source projects to work properly:
* For frontend:
    * [React & Redux] with TypeScript
    * [Material UI]
    * [Jest] for unit testing

* For backend:
    * [Node.js]
    * [MongoDB with Mongoose]
    * [Mocha] for unit testing

# Backend
Backend application states under **/backend/{*Confidential*}-api**
This is a basic REST API contains the needed enpoints.

### Configuration
* **Environment Configuration**
    You need to configure these two variables on your environment:
    ```sh
    MONGODB_URI=<YOUR_MONGO_DB_CONNECTION_URL_STRING>
    PORT=<APPLICATION_PORT>
    ```
    To make the API run on DEV environment, add **.env.dev** file under **/config** folder with the definitions above.
    And change **start** script on **package.json** with setting **NODE_ENV** to dev
    ```sh
    "scripts": {
        "start": "set NODE_ENV=dev&& node app.js",
    ```
    For production, application will get those values from your environment.
    
* **Migration**
    For permission and role tables exported under **/migration** folder, you can import them.
     ```sh
    "permissions-migration.json"
    "roles-migration.json"
    ```
    On **/models/constants**, you need to configure the role type name exactly on your database,
    application will use them on **Authorization** layer.
    ```sh
    Role: {
        SuperAdmin:"SUPER_ADMIN",
        Admin:"ADMIN",
        Owner:"OWNER",
        Regular:"REGULAR"
    }
    ```
* **API Endpoints**
    Api Endpoints are under **/models/constants**, you can configure the enpoint names in there:
    ```sh
    ApiEndpoint: {
        PATH:"/api/v1/",
        USER:"users",
        AUTH:"auths",
        ...,
        SWAGGER:"api-docs"
    }
    ```

### Installation
Application requires [Node.js] to run.
Install the dependencies and start the server.
```sh
$ cd backend/{*Confidential*}-api
$ npm install
$ npm start
```

### Plugins
| Plugin | Purpose |
| ------ | ------ |
| [Jwt] | To generate token and verification |
| [Mongoose] | To handle repository layer operations |
| [Swagger] | To document API |
| [Mocha] | To runs test |

### Architecture
**Model** - **Routes** - **Controller** architecture is built for this API.

##### Layers

* **Models** contains the database schema providers, like **User** schema, **Role** schema..
* **Routes** contains the routes for the endpoints, and builds which enpoint will use which controller method. 
    For Example this is User Router, and on patch operation it calls updateUser method from controller;
    ```sh
    userRouter.patch('/:id', userController.updateUser);
    ```
* **Controller** contains the methods that applies request, like running a method on repository layer and sends the response
* **Repository** contains the data layer operations like get user from database, save user to database..
* **Middleware** contains middleware providers, like data proiver (database connection provider), token provider, 
    encryption handler, logger..
* **Authorization** authorization layer is managed by **PermissionController.js** under **/controllers**
* **Authentication** authentication layer is managed by **AuthController.js** under **/controllers**

#### Token Operations

* [Jwt] is used for this API to manage token operations
* **secret** is configurable under **/config/Config.js** 
    ```sh
    module.exports = {
        secret: 'what a wicked game to play'
    ```
* **TokenProvider.js** under **/middleware** manages the all token operations like generate token, verify token..

#### Tests
* Unit & Integration tests are handled using [Mocha]
    You can find and extend them on the folder **/tests/mocha**

    To run mocha tests and coverage, you can use this command:
    ```sh
    $ npm test
    ```
    
* Regression tests are handled using [Postman]
    All endpoint request scenarios are covered. You can import the tests:
    ```sh
    $ cd /backend/{*Confidential*}-api/tests/postman
    "toptal-{*Confidential*}-api.postman_collection.json"
    ```
    And for the environment variables, you can import;
    ```sh
    $ cd /backend/{*Confidential*}-api/tests/postman
    "toptal-{*Confidential*}.local.postman_environment.json"
    ```
    You should re-configure the values in there according to your environment, like API host, port, Owner user id on your db etc.

#### Dependency Injection
All classes are  **Layer** based injectible.

* All **middleware providers** are injectible.
* All **models** are injectible for **repositories**
* All **repositories** are injectible for **controllers**
* All **controllers** are injectible for **routes**

#### CI / CD
CI / CD operations are handled by [Gitlab CI]
To setup it, ```
    .gitlab-ci.yml
    ``` file prepared.

To run pipeline on local, you will need [Gitlab Runner].
* Two jobs are defined for backend builds & tests, you can run them with commands:
    ```sh
    gitlab-runner exec shell build_backend
    gitlab-runner exec shell test_backend
    ```

#### Containerization
Containerization is handled with [Docker]. Pushed Dockerfile example to git to take a reference on your builds.
* Go to backend directory. Run these commands:
```sh
docker build -t <your username>/{*Confidential*}-api:latest .
```
* After successful build check the image is created:
```sh
docker images
```
* Run the container:
```sh
docker run -d -p 3000:3000 <your username>/{*Confidential*}-api
```
* And check the container is up:
```sh
docker ps
```
    
#### Documentation
API is documented using [Swagger].

* When you run the API, you can reach the documentation via url
    ```sh
    host:port/api/v1/api-docs
    ```
* This path is configurable under **/models/Constant.js** ApiEndpoint
* You can edit this document on [Swagger Editor] via using
    ```sh
    cd backend/{*Confidential*}-api/docs/swagger
    "swagger.yaml"
    ```
    
# Frontend
Frontend application states under **/frontend/{*Confidential*}**
This is a single page application contains the needed functionality to cover the requirements.
Implemented with using [React & Redux] and design is handled with [Material UI].

### Installation
Application requires [Node.js] to run.
Install the dependencies and start the server.
```sh
$ cd frontend/{*Confidential*}
$ npm install
$ npm start
```

### Configuration
* You need to configure api gateways and url on **/config/index.ts**:
    ```sh
    apiGateway: {
        URL: "YOUR_PROD_API_GATEWAY_URL",
        AUTH_ENDPOINT: "YOUR_PROD_AUTH_ENDPOINT_URL",
        ...
    }
    ```
* You need to configure role ids in your database on **/config/index.ts**:
    ```sh
    role: {
        SUPER_ADMIN: "SUPER_ADMIN_ROLE_ID_ON_YOUR_DATABASE",
        ADMIN: "ADMIN_ROLE_ID_ON_YOUR_DATABASE",
        OWNER: "OWNER_ROLE_ID_ON_YOUR_DATABASE",
        REGULAR: "REGULAR_ROLE_ID_ON_YOUR_DATABASE"
    }
    ```
    To make the API run on DEV environment;
    Change **start** script on **package.json** with setting **REACT_APP_STAGE** to dev
    ```sh
    "scripts": {
        "start": "set REACT_APP_STAGE=dev&& react-scripts start",
    ```

### Plugins
| Plugin | Purpose |
| ------ | ------ |
| [Redux] | To handle store management |
| [Jest] | To runs test |
| [Material UI] | To handle design |

### Architecture
Basic Redux architecture is built for this frontend.

##### Layers
* **Types** contains declaration for the action types
* **Actions** contains implementation for actions like sending Api CRUD requests, and dispatching data to reducers
* **Reducer** contains the reducers for state management after getting new state from action dispatches
* **ViewModels** contains specific models for views
* **Models** contains models for objects like User, Review, Restaurant..
* **Components** contains components for the application
* **Store** combines reducers
* **Router** contains all routes for the application

#### Tests
* Unit & Integration tests are handled using [Jest]
    To run jest tests and coverage, you can use this command:
    ```sh
    $ npm test
    ```
* Your test files naming should be ```*.test.ts``` to run them with **npm test** command

#### CI / CD
CI / CD operations are handled by [Gitlab CI]
To setup it, ```
    .gitlab-ci.yml
    ``` file prepared.

To run pipeline on local, you will need [Gitlab Runner].
* Two jobs are defined for frontend builds & tests, you can run them with commands:
    ```sh
    gitlab-runner exec shell build_frontend
    gitlab-runner exec shell test_frontend
    ```

#### Containerization
Containerization is handled with [Docker]. Pushed Dockerfile example to git to take a reference on your builds.
* Go to frontend directory. Run these commands:
```sh
docker build -t <your username>/{*Confidential*}:latest .
```
* After successful build check the image is created:
```sh
docker images
```
* Run the container:
```sh
docker run -d -p 5000:5000 <your username>/{*Confidential*}
```
* And check the container is up:
```sh
docker ps
```

[Docker]:<https://www.docker.com/>
[Material UI]:<https://material-ui.com/>
[React & Redux]:<https://redux.js.org/basics/usage-with-react>
[Redux]:<https://redux.js.org/basics/usage-with-react>
[Jest]:<https://jestjs.io/>
[MongoDB with Mongoose]:<https://mongoosejs.com/>
[Node.js]: <http://nodejs.org>
[Mocha]:<https://mochajs.org/>
[Jwt]:<https://jwt.io/>
[Mongoose]:<https://mongoosejs.com/>
[Swagger]:<https://swagger.io/>
[Postman]:<https://www.getpostman.com/>
[Gitlab CI]:<https://about.gitlab.com/product/continuous-integration/>
[Gitlab Runner]:<https://docs.gitlab.com/runner/>
[Swagger Editor]:<https://editor.swagger.io/>
