# Test cases for API [Swagger Petstore](https://petstore.swagger.io/#/)

Test coverage: Pet controller, endpoint POST /pet

*Test cases created only for JSON content type. 
XML content type is out of the scope.*

### 1. It is possible to create a pet 
Priority: High 

**Steps:**
1. Set Header Content-Type with value `application/json`
2. Add request body in JSON format with valid parameters:
```
{
    "id": 123451,
    "category": {
        "id": 1,
        "name": "dog"
    },
    "name": "Lucky",
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ],
    "tags": [
        {
            "id": 1,
            "name": "Fluffy"
        }
    ],
    "status": "available"
}
```
3. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 200
* Response content type is equal to `application/json`
* Response body parameters equal to requested
* Response body structure equal to required [Schema](#response-schema)

**Postconditions:** Delete created pet with id `123451` using request DELETE /pet/{petId}

### 2. Pet can be created when sending only required parameters
Priority: High 

**Steps:**
1. Set Header Content-Type with value `application/json`
2. Add request body in JSON format with only required parameters valid values:
```
{
    "name": "Waffle",
    "photoUrls": [
        "http://placeimg.com/640/480/waffle-dog"
    ]
}
```
3. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 200
* Response content type is equal to `application/json`
* Response body parameter *id* exists and have integer value 
* Response body parameters *name* and *photoUrls* have same values that in request body
* Response body parameter *tags* exists and an empty array

**Postconditions:** Delete created pet using request DELETE /pet/{petId}

### 3. Pet can be created with ‘sold’ status 
Priority: Medium

**Steps:**
1. Set Header Content-Type with value `application/json`
2. Set value of parameter 'status' set as 'sold'
3. Add other parameters to request body:
```
{
    "id": 123453,
    "name": "Lucky",
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ]
    "status": "sold"
}
```
4. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 200
* Response content type is equal to `application/json`
* Response body parameter 'status' have value 'sold'

**Postconditions:** Delete created pet with id `123453` using request DELETE /pet/{petId}

### 4. Pet can not be created if required parameter ‘name‘ is missing 
Priority: High
**Steps:**
1. Set Header Content-Type with value `application/json`
2. Add request body in JSON format without 'name' parameter:
```
{
    "id": 123454,
    "category": {
        "id": 1,
        "name": "dog"
    },
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ],
    "tags": [
        {
            "id": 1,
            "name": "Fluffy"
        }
    ],
    "status": "available"
}
```
3. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "validation",
    "message": "Required parameter `name` is missing. Please check your data"
```

### 5. Pet can not be created if parameter have invalid value
Priority: High
**Steps:**
1. Set Header Content-Type with value `application/json`
2. In request body set to parameter `id` value `test`
3. Add other required parameters to request body:

```
{
    "id": "123455",
    "name": "Lucky",
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ]
}
```
4. Send POST request https://petstore.swagger.io/v2/pet
**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "validation",
    "message": "Parameter `id` is invalid. Please check your data'"
}
```

### 6. Pet can’t be created with null value of parameter 'name'
Priority: High
**Steps:**
1. Set Header Content-Type with value `application/json`
2. In request body set to parameter `name` value `null`
3. Add other required parameters to request body:

```
{
    "name": null,
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ]
}
```
4. Send POST request https://petstore.swagger.io/v2/pet
**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "validation",
    "message": "Parameter `name` is invalid. Please check your data'"
}
```
### 7. Pet can not be created if empty request send 
Priority: High
**Steps:**
1. Set Header Content-Type with value `application/json`
2. Add an empty JSON body
3. Send POST request https://petstore.swagger.io/v2/pet
**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "validation",
    "message": "Request body is missing or empty."
}
```
### 8. Pet can not be created with duplicated id 
Priority: High
**Preconditions:** Pet with id `123458` already exist in the system.
**Steps:**
1. Set Header Content-Type with value `application/json`
2. In request body set to parameter `id` value `123458`
3. Add other required parameters to request body:
```
{
    "id": 123458,
    "name": "Dora",
    "photoUrls": [
        "http://placeimg.com/640/480/dora-cat"
    ]
}
```
4. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "conflict",
    "message": "A resource with the provided ID already exists."
}
```

### 9. Pet can not be created with status out of valid range 
Priority: High
**Steps:**
1. Set Header Content-Type with value `application/json`
2. In request body set to parameter `status` value `unavailable`
3. Add other required parameters to request body:
```
{
    "name": "Dora",
    "photoUrls": [
        "http://placeimg.com/640/480/dora-cat"
    ],
    "status": "unavailable"
}
```
4. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 400
* Respose body equals to:
```
{
    "code": 400,
    "type": "validation",
    "message": "Invalid value for parameter 'status'."
}
```

### 10. Response Time for pet creation endpoint less than 1 sec
Priority: High 

**Steps:**
1. Set Header Content-Type with value `application/json`
2. Add request body in JSON format with valid parameters:
```
{
    "id": 1234510,
    "category": {
        "id": 1,
        "name": "dog"
    },
    "name": "Lucky",
    "photoUrls": [
        "http://placeimg.com/640/480/lucky-dog"
    ],
    "tags": [
        {
            "id": 1,
            "name": "Fluffy"
        }
    ],
    "status": "available"
}
```
3. Send POST request https://petstore.swagger.io/v2/pet

**Expected result:**
* Response status code is 200
* Response time is < 1s

**Postconditions:** Delete created pet with id `1234510` using request DELETE /pet/{petId}

<a id="response-schema"> Pet response structure:
```
"type": "object",
        "properties": {
            "id": {
                "type": "number"
            },
            "category": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "number"
                    },
                    "name": {
                        "type": "string"
                    }
                },
                "required": ["id", "name"]
            },
            "name": {
                "type": "string"
            },
            "photoUrls": {
                "type": "array",
                "items": {
                    "type": "string"
                }
            },
            "tags": {
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "id": {
                            "type": "number"
                        },
                        "name": {
                            "type": "string"
                        }
                    },
                    "required": ["id", "name"]
                }
            },
            "status": {
                "type": "string"
            }
        },
        "required": ["id", "category", "name", "photoUrls", "tags", "status"]
    };
```
</a>
