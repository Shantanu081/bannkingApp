# MoneyTransfer

Developed using Java 8 and [Vert.x](http://vertx.io).

## Features
* Account creation/update/deletion
* Transfer creation
* Transfer execution
* Meaningful HTTP return codes
* In-memory datastore
* Stand-alone jar (no need for a pre-installed container/server)
* Integration tests (using [REST-assured](http://rest-assured.io))

## Requirements
* Java 8
* Maven

## How to build the application
Checkout the project from this repository, then run
```
    mvn clean package
```
## How to run tests
Checkout the project from this repository, then run
```
    mvn clean verify
```
## How to run the application
Build the application or use the [fat jar], then run
```
    java -jar moneytransfer-1.0-SNAPSHOT-fat.jar
```
The application runs on port 8080

##To Run the application please follow :

1. From the root folder run ‘mvn clean install’
2. Then run, ‘docker build -t revolut .’
3. docker image ls (to check the created image)
4. docker ps -a (to check status)
5. docker run -p 8080 revolut (Run the app)
6. Hit URL from postman.

## How to use the application
### Accounts
#### Create an account
The following request creates an account and returns it:
```
    POST localhost:8080/api/accounts
    {
        "name": "Shantanu",
        "balance": "4325",
        "currency": "EUR"
    }
```
Response:
```
    HTTP 201 Created
    {
        "id": 0,
        "name": "Shantanu",
        "balance": "4325",
        "currency": "EUR"
    }
```
#### Update an account
The following request updates an account and returns it:
```
    PUT localhost:8080/api/accounts/0
    {
        "name": "Shantanu",
        "currency": "USD"
    }
```
Response:
```
    HTTP 200 OK
    {
        "id": 0,
        "name": "Shantanu",
        "balance": "4325",
        "currency": "USD"
    }
```
* Only the values of "name", "currency" and "balance" can be updated
* If one or more value is invalid, the request will fail

#### Delete an account
The following request deletes an account ('0' is the primary Id):
```
    DELETE localhost:8080/api/accounts/0
```
Response:
```
    HTTP 204 No Content
```
#### Retrieve all accounts
The following request retrieves all accounts in the datastore(Account details are dummy here, for real application the value might change)
```
    GET localhost:8080/api/accounts
```
Response:
```
    HTTP 200 OK
    [
        {
            "id": 0,
            "name": "Account 1",
            "balance": 2345,
            "currency": "EUR"
        },
        {
            "id": 1,
            "name": "Account 2",
            "balance": 437,
            "currency": "EUR"
        },
        {
            "id": 2,
            "name": "Account 3",
            "balance": 10000,
            "currency": "GBP"
        }
    ]
```
#### Retrieve one account
The following request retrieves one account in the datastore(Account details are dummy here, for real application the value might change)
```
    GET localhost:8080/api/accounts/0
```
Response:
```
    HTTP 200 OK
    {
        "id": 0,
        "name": "Account 1",
        "balance": 2345,
        "currency": "EUR"
    }
```
### Transfers
#### Create a transfer
The following request creates a transfer and returns it:
```
    POST localhost:8080/api/transfers
    {
        "sourceAccountId": "0",
        "destinationAccountId": "1",
        "amount": "450",
        "currency": "EUR",
        "comment": "Rent"
    }
```
Response:
```
    HTTP 201 Created
    {
        "id": 0,
        "sourceAccountId": 0,
        "destinationAccountId": 1,
        "amount": 450,
        "currency": "EUR",
        "comment": "Rent",
        "status": "CREATED"
    }
```
#### Try to execute a transfer
The following request tries to execute a transfer and returns it:
```
    PUT localhost:8080/api/transfers/0
```
Response:
```
    HTTP 200 OK
    {
        "id": 0,
        "sourceAccountId": 0,
        "destinationAccountId": 1,
        "amount": 450,
        "currency": "EUR",
        "comment": "Rent",
        "status": "EXECUTED"
    }
```
A transfer is successfully executed if:
* The source and destination accounts exists
* There is a sufficient balance on the source account
* The currency of the transfer, of the source and destination accounts are identical
* The transfer status is not FAILED or EXECUTED

If one or more of these conditions are not fulfilled, the transfer status is set to FAILED.
