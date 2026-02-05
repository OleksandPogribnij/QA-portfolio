# TEST CASES EXAMPLE

### Assumptions:

Test cases written for the Petstore API, specifically for the /user endpoint (POST).
Since the Swagger Petstore documentation does not contain clear requirements for field validation, I rely on generally accepted data security and quality standards in the test cases (e.g., username uniqueness, email requirement, etc.).

---
### TC_USER_001 | Create user with a valid credentials

**Priority**: High

**Method**: POST

**Endpoint**: /user

**Test Type**: Positive

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;
- There is no user with such credentials in the system;

#### Steps to reproduce:
1. Send POST request with valid request body;

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```

#### Expected Result:
- Response status code is 200 (or 201 Created);
- User is created successfully. The entry has been added to the database;
- The response body contains:
  - the “status” field contains the 200 status code;
  - the “message” field contains the correct user ID that was sent in the request;

---
### TC_USER_002 | Checking user credentials uniqueness

**Priority**: High

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative / Business logic

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;
- The system(database) contains a user with the specified username and email address;

#### Steps to reproduce:
1. Send POST request with valid request body (but with existing username/email);

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```

#### Expected Result:
- Response status code is 400 (or 409 Conflict);
- The response body contains “message” error description field;
- User not created;

#### Note: The current implementation of Petstore incorrectly allows duplicate users.
---
### TC_USER_003 | Checking the validation of the email format

**Priority**: Medium

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative 

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;
- There is no user with such credentials in the system;

#### Steps to reproduce:
1. Send POST request with non-valid email (for example "tester");

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:

1. 
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```
2. 
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "@gmail.com",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```
3. 
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "testergmail.com",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```

#### Expected Result:
- Response status code is 400;
- Response body contains a clear error message: (for example) "Invalid email format".
- User not created;
---
### TC_USER_004 | Checking password length and character validation

**Priority**: High

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative / Validation

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;
- There is no user with such credentials in the system;

#### Steps to reproduce:
1. Send POST request:

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:

1. 
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "1",
  "phone": "+380000000000",
  "userStatus": 1
}
```
2. Buffer overflow check (or database limits)
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!-Password!",
  "phone": "+380000000000",
  "userStatus": 1
}
```
3. 
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "Tester",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "password123",
  "phone": "+380000000000",
  "userStatus": 1
}
```

#### Expected Result:
- Response status code is 400;
- Validation error in the “password” field;
- User not created;
---
### TC_USER_005 | Checking data types

**Priority**: Medium

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;
- There is no user with such credentials in the system;

#### Steps to reproduce:
1. Send POST request with incorrect data formats;

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:

1. Incorrect data format for the "id" field:
```
{
  "id": "string_id",
  ...
}
```
2. Incorrect data format for the "firstName" field:
```
{
  ...
  "firstName": 123,
  ...
}
```
3. Incorrect data format for the "lastName" field:
```
{
  ...
  "lastName": 123,
  ...
}
```
4. Incorrect data format for the "email" field:
```
{
  ...
  "email": 312,
  ...
}
```
5. Incorrect data format for the "password" field:
```
{
  ...
  "password": 12345678,
  ...
}
```
6. Incorrect data format for the "phone" field:
```
{
  ...
  "phone": 234,
  ...
}
```
7. Incorrect data format for the "userStatus" field:
```
{
  ...
  "userStatus": "string_status"
}
```

#### Expected Result:
- Response status code is 400;
- Validation error of the filled in with incorrect data field;
- User not created;
---
### TC_USER_006 | Checking the sending of an empty request body

**Priority**: Medium

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;

#### Steps to reproduce:
1. Send POST request with an empty request body;

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:

1. 
```
{
  
}
```
#### Expected Result:
- Response status code is 400;
- User not created;
---
### TC_USER_007 | Check security (XSS Injection)

**Priority**: HIGH

**Method**: POST

**Endpoint**: /user

**Test Type**: Negative / Secure

**Environment**: https://petstore.swagger.io/

#### Preconditions:
- Swagger Petstore API is available;

#### Steps to reproduce:
1. Send POST request:

#### Test Data:

Request: `https://petstore.swagger.io/v2/user`

Body:
```
{
  "id": 123,
  "username": "test_name",
  "firstName": "<script>alert(1)</script>",
  "lastName": "QA",
  "email": "tester@gmail.com",
  "password": "Password123!",
  "phone": "+380000000000",
  "userStatus": 1
}
```

#### Expected Result:
- Response status code is 200. The value is stored in the database.
- The API should either reject the request with 400 or sanitize(or escape) characters (<, >) in the stored value. In the response, the script must not be executed by a client browser.
- User created succesfully;
---

