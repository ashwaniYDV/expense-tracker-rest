# expense-tracker-spring-rest

A RESTful API for tracking expenses created using Spring Boot.

Used PostgreSQL as the relational database and JdbcTemplate to interact with that.

Used JSON Web Token (JWT) to add authentication and protect certain endpoints.

Users can register and login and then record their expense transactions under different categories.

Authenticated users can create various categories such as for shopping, food items, billings and then for eath of these categories they can add transactions.

This is a typical OneToMany relationship.

## Setup and Installation

1. **Clone the repo from GitHub**
   ```sh
   git clone https://github.com/ashwaniYDV/expense-tracker-spring-rest.git
   cd expense-tracker-spring-rest
   ```
2. **Spin-up PostgreSQL database instance**

   You can use either of the below 2 options:
   - one way is to download from [here](https://www.postgresql.org/download) and install locally on the machine
   - another option is by running a postgres docker container:
     ```sh
     docker container run --name postgresdb -e POSTGRES_PASSWORD=admin -d -p 5432:5432 postgres
     ```
3. **Create database objects**

   In the root application directory (expense-tracker-spring-rest), SQL script file (expensetracker_db.sql) is present for creating all database objects
   - if using docker (else skip this step), first copy this file to the running container using below command and then exec into the running container:
     ```
     docker container cp expensetracker_db.sql postgresdb:/
     docker container exec -it postgresdb bash
     ```
   - run the script using psql client:
     ```
     psql -U postgres --file expensetracker_db.sql
     ```
4. **(Optional) Update database configurations in application.properties**
   
   If your database is hosted at some cloud platform or if you have modified the SQL script file with some different username and password, update the src/main/resources/application.properties file accordingly:
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/expensetrackerdb
   spring.datasource.username=expensetracker
   spring.datasource.password=password
   ```
5. **Run the spring boot application**
   ```sh
   ./mvnw spring-boot:run
   ```
   this runs at port 8080 and hence all enpoints can be accessed starting from http://localhost:8080

## API Schema

#### /api/users
| Route      | Method   | Description                     | Authorization      |
|------------|----------|---------------------------------|--------------------|
| /register  | [POST]   | user register                   | (No)               |
| /login     | [POST]   | user login                      | (No)               |


#### /api/categories
| Route        | Method   | Description           | Authorization |
|--------------|----------|-----------------------|---------------|
| /            | [POST]   | creating a category for authenticated user    | (Yes)      |
| /            | [GET]    | get all categories of authenticated user      | (Yes)       |
| /:categoryId | [GET]    | get catagory by Id of authenticated user | (Yes)       |
| /:categoryId | [PUT]    | Update catagory by Id of authenticated user | (Yes)       |
| /:categoryId | [DELETE] | Delete catagory by Id and also deletes all transactions under this category of authenticated user| (Yes) |

#### /api/categories/:categoryId/transactions
| Route                        | Method   | Description                             | Authorization   |
|------------------------------|----------|-----------------------------------------|-----------------|
| /                            | [GET]    | get all transactions by categoryId of authenticated user  | (Yes)        |
| /                            | [POST]    | create a transactions by categoryId of authenticated user  | (Yes)        |
| /:transactionId              | [GET]    | get a transactions by categoryId and transactionId of authenticated user  | (Yes)   |
| /:transactionId              | [PUT]    | update a transactions by categoryId and transactionId of authenticated user  | (Yes)   |
| /:transactionId              | [DELETE]    | delete a transactions by categoryId and transactionId of authenticated user  | (Yes)   |


#### Learnt from

https://www.youtube.com/watch?v=fVq9aPNGLAg&list=PLWieu6NbbqTwwYwylgXmmKVX1ZWsUVx8m
