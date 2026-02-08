# API Test Checklist: Swagger Petstore (Pet Resource)

This checklist covers the main testing scenarios for the `/pet` endpoint, including positive, negative, and boundary checks.

---

## [POST /pet](https://petstore.swagger.io/#/pet/addPet) (Add a new pet to the store)
- [ ] **Positive:** Creating a pet with all valid fields. (**Expected result**: 200 OK)
<details>
  <summary>Click to view the body of the request</summary>

```json
{
  "id": 1025,
  "category": {
    "id": 1,
    "name": "Dogs"
  },
  "name": "Molly",
  "photoUrls": [
    "https://example.com/molly.jpg"
  ],
  "tags": [
    {
      "id": 12,
      "name": "friendly"
    }
  ],
  "status": "available"
}
```
</details>

- [ ] **Required fields:** Attempt to create an animal without a `name` field (expected: 400 error).

<details>
  <summary>Click to view the body of the request</summary>

```json
{
  "id": 1025,
  "category": {
    "id": 1,
    "name": "Dogs"
  },
  "name": null,
  "photoUrls": [
    "https://example.com/molly.jpg"
  ],
  "tags": [
    {
      "id": 12,
      "name": "friendly"
    },
    {
      "id": 15,
      "name": "vaccinated"
    }
  ],
  "status": "available"
}
```
</details>

- [ ] **Data types:** Checking the string passed to the `id` field (expected: validation error).

<details>
  <summary>Click to view the body of the request</summary>

```json
{
  "id": "string",
  "category": {
    "id": 1,
    "name": "Dogs"
  },
  "name": "Molly",
  "photoUrls": [
    "https://example.com/molly.jpg"
  ],
  "tags": [
    {
      "id": 12,
      "name": "friendly"
    },
    {
      "id": 15,
      "name": "vaccinated"
    }
  ],
  "status": "available"
}
```
</details>

- [ ] **Limit values:** Creating an animal with a very long `name` value (e.g., 1000+ characters).

<details>
  <summary>Click to view the body of the request</summary>

```json
{
  ...
  "name": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Curabitur pretium tincidunt lacus. Nulla gravida orci a odio. Nullam varius, turpis et commodo pharetra, est eros bibendum elit, nec luctus magna felis sollicitudin mauris. Integer in mauris eu nibh euismod gravida. Duis ac tellus et risus vulputate vehicula. Donec lobortis risus a elit. Etiam tempor. Ut ullamcorper, ligula eu tempor congue, eros est euismod turpis, id tincidunt sapien risus a quam. Maecenas fermentum consequat mi. Donec fermentum. Pellentesque malesuada nulla a mi. Duis sapien sem, aliquet nec, commodo eget, consequat quis, neque. Aliquam faucibus.",
  ...
}
```
</details>

- [ ] **Array validation:** Create a record with multiple objects in `tags` and multiple links in `photoUrls` (expected status code: 200 OK).

<details>
  <summary>Click to view the body of the request</summary>

```json
{
  "id": 1025,
  "category": {
    "id": 1,
    "name": "Dogs"
  },
  "name": "Molly",
  "photoUrls": [
    "https://example.com/molly.jpg",
    "https://example.com/molly1.jpg"
  ],
  "tags": [
    {
      "id": 12,
      "name": "friendly"
    },
    {
      "id": 15,
      "name": "vaccinated"
    }
  ],
  "status": "available"
}
```
</details>

---

## [POST /pet/{petId}/uploadImage](https://petstore.swagger.io/#/pet/uploadFile) (Image upload)
- [ ] **File formats:** Uploading files in the following formats `.jpg`, `.png`, `.gif`.

<details>
  <summary>Click to view the request</summary>

```
curl -X 'POST' \
  'https://petstore.swagger.io/v2/pet/1025/uploadImage' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@/example/molly.png'
```
</details>

- [ ] **Invalid formats:** Attempt to download `.txt` or `.exe` instead of an image.

<details>
  <summary>Click to view the request</summary>

```
curl -X 'POST' \
  'https://petstore.swagger.io/v2/pet/1025/uploadImage' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@/example/molly.txt'
```
</details>

- [ ] **ID validation:** Loading image for non-existent `petId` (expected: 404).

<details>
  <summary>Click to view the request</summary>

```
curl -X 'POST' \
  'https://petstore.swagger.io/v2/pet/999999999/uploadImage' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@/example/molly.png'
```
</details>

- [ ] **Metadata:** Checking the transfer of additional text description in the `additionalMetadata` parameter.

<details>
  <summary>Click to view the request</summary>

```
curl -X 'POST' \
  'https://petstore.swagger.io/v2/pet/999999999/uploadImage' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'additionalMetadata=Test upload' \
  -F 'file=@/example/molly.png'
```
</details>

---

## [GET /pet](https://petstore.swagger.io/#/pet/getPetById) (Searching for and obtaining data)
- [ ] **Search by ID:** Obtaining data on existing animals (Expected result: 200 OK).

<details>
  <summary>Click to view the request</summary>

```
GET https://petstore.swagger.io/v2/pet/1025
```
</details>

- [ ] **Non-existent ID:** Request for data by `petId` that does not exist in the database (expected: 404 Not Found).

<details>
  <summary>Click to view the request</summary>

```
GET https://petstore.swagger.io/v2/pet/9999999
```
</details>

## [GET /pet/findByStatus](https://petstore.swagger.io/#/pet/findPetsByStatus) (Search for a pet by status)
- [ ] **Filter by status:** Checking each status separately: `available`, `pending`, `sold`.
- [ ] **Multiple status:** Checking a request with multiple statuses simultaneously (for example, `status=available&status=pending`).
- [ ] **Invalid status:** Request for status `test_status` (expected: empty list or error, but not 500 status code).

---

## PUT & POST /pet (Data update)
- [ ] **Complete update (PUT):** Changing all attributes of an existing animal.

<details>
  <summary>Click to view the body of the request</summary>

```
PUT https://petstore.swagger.io/v2/pet
```

```json
{
  "id": 1025,
  "category": {
    "id": 1,
    "name": "Cats"
  },
  "name": "Billy",
  "photoUrls": [
    "https://example.com/billy.jpg"
  ],
  "tags": [
    {
      "id": 22,
      "name": "angry"
    }
  ],
  "status": "available"
}
```
</details>

- [ ] **Partial update via Form Data:** Change only the name (`name`) or status (`status`) using `POST /pet/{petId}`.
- [ ] **Consistency:** Checking whether other fields are overwritten during partial updates via the form.

---

## DELETE /pet (Deleting)
- [ ] **Deleting:** Deleting an existing object and checking the 200/204 response code.

<details>
  <summary>Click to view the request</summary>

```
  DELETE 'https://petstore.swagger.io/v2/pet/1025'
```
</details>

- [ ] **Repeat deletion:** Attempt to delete the same `id` a second time (idempotency check).

<details>
  <summary>Click to view the request</summary>

```
  DELETE 'https://petstore.swagger.io/v2/pet/1025
```
</details>

- [ ] **Verification:** Execution of the `GET /pet/{petId}` request after deletion (should return 404).

<details>
  <summary>Click to view the request</summary>

```
  GET 'https://petstore.swagger.io/v2/pet/1025
```
</details>

- [ ] **Safety:** Attempt to delete an object without passing `api_key` (if access restrictions are configured).

---

## General technical inspections (Across all endpoints)
- [ ] **Response Headers:** Availability check `Content-Type: application/json`.
- [ ] **Performance:** The server response time does not exceed 500 ms for GET requests.
- [ ] **Error Handling:** Checking whether the server returns understandable error messages in JSON format.
- [ ] **CORS:** Checking for headings `Access-Control-Allow-Origin`.
