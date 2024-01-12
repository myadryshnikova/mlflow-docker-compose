# MLflow On-Premise Deployment using Docker Compose
Easily deploy an MLflow tracking server with 1 command.

## How to run
1. Clone this repository

    ```bash
    git clone https://github.com/myadryshnikova/mlflow-docker-compose.git
    ```

2. `cd` into the `mlflow-docker-compose` directory

3. `Optional`: you can change credentials for services in `.env` file. For this:
    ```bash
    nano .env
    ```
    In this file, the following variables are available for you to modify:
    * AWS_ACCESS_KEY_ID
    * AWS_SECRET_ACCESS_KEY
    * MYSQL_DATABASE
    * MYSQL_USER
    * MYSQL_PASSWORD
    * MYSQL_ROOT_PASSWORD

4. Build and run the containers with `docker-compose`

    ```bash
    docker-compose up -d --build
    ```

## Containerization
The MLflow tracking server is composed of 4 docker containers:

* MLflow server
* MinIO object storage server
* MySQL database server

## Port Map
| Service                      | Port | 
|------------------------------|------|                  
| db                           | 3306 |              
| mlflow-server                | 5000 |
| minio REST                   | 9000 |                  
| minio UI                     | 9001 |

Locally, services will be accessed at http://localhost:<port>. 
If deployed on a server and must be accessed externally, services will be accessed at http://<server ip-address>:<port>. 

To change the ports for services, you need to change them in the `docker-compose.yml` file.

## Mlflow authentication
Basic authentication to mlflow-server is enabled by default in this project.

Default credentials of the service admin:
* User: `admin`
* Password: `password`


After starting the service, you should immediately change the password for the administrator. This can be done using the REST request described in the next section.

Further explanations will be linked to the Mlflow documentation. You can see more details of the endpoints at the link: 
https://www.mlflow.org/docs/latest/auth/index.html#authenticating-to-mlflow

### Quick start on important endpoints in service deployment
This section describes the endpoints needed to get started with user management and experimentation. More in-depth information can be found in the MLflow documentation.

Endpoints are available at the address of the deployed mlflow-server: 
`http://<server ip-address>:5000/api/`. HTTP authentication by **admin** credentials is required for all endpoints.

| API                          | Endpoint                                  | Method| Request scheme | 
|------------------------------|-------------------------------------------|-------| ---------------|                
| Update User Password         | 2.0/mlflow/users/update-password          | PATCH |json<br><pre>{<br>  "username": str,<br>  "password": str<br>}</pre>|
| Create User                  | 2.0/mlflow/users/create                   | POST  |json<br><pre>{<br>  "username": str,<br>  "password": str<br>}</pre>|  
| Update User Admin            | 2.0/mlflow/users/update-admin             | PATCH |json<br><pre>{<br>  "username": str,<br>  "is_admin": bool<br>}</pre>|              