## REST API Design

While designing an REST API it is important to make sure we create path for URL ( uniform resource locator ) should only contains resources ( nouns ) not actions ( verbs ). For example: If we are wanting to create new student in a school then typical path would be `/addNewStudent`, we can make use of HTTP methods (verbs) to do the same by just using the resources
```
POST '/students'
or
POST '/schools/12/students'
```
In first case, we are just using HTTP verb to create new student. In second case, we are creating new student for school of ID number 3. ** Note all resources are plural.

### HTTP Verbs

#### GET
Used to request data from the server and should not produce any side effects. Successful response is returned with the status code `200`. Example usage would be,
```
GET '/schools/3/students'
```
this would return all students from school which has an ID 3.

#### POST
Used to create a resource in a database, typically called when the new form is submitted. Once the resource is created successfully then status code `201` is returned. This has `NON-IDEMPOTENT` effect on the multiple request, i.e if the multiple request is requested by the client then we will have duplicate records for the same. Example usage would be,
```
POST '/schools/3/students'
```
this would create new student for school which has an id 3.

#### PUT
Used to update the resource or create if the requested resource is not found. This is `INDEMPOTENT`, so if the request has been made multiple times then the subsequent request will have not effect. Example usage would be,
```
PUT '/schools/3/students/bob'
```
this will try to update the student name `bob` if not found then it will create new student by name bob.

#### DELETE
Is the same way as PUT and will delete the resource from database. We get this http status code when we get `204 No Content` response. That means the request has been processed successfully and no response is requested. However if the resource is not found in the database then we get `404 Resource Not Found`. The problem with DELETE, which if successful would normally return a 200 (OK) or 204 (No Content), will often return a 404 (Not Found) on subsequent calls.

### Important HTTP Status

1. 200 - Successful message returned from `GET`, `PUT` or `POST`.
2. 201 - Successfully created returned from `POST`.
3. 204 - No Content returned from successful `DELETE` operation.
4. 304 - Not modified returned if the data is cached and no extra updated needed.
5. 400 - Bad Request thrown from the server if the request is not understood.
6. 401 - Unauthorized, indicates the client is not allowed to access the resource.
7. 403 - Forbidden, indicates the request is valid and client is authenticated but client is not allowed to process it.
8. 404 - Not available.
9. 500 - Internal server error, indicates valid request but server couldn't process it.
10. 503 - Service Unavailable, indicates the server is down.

IMP: [HTTP STATUS CODE](https://www.restapitutorial.com/httpstatuscodes.html)



Reference:
1. https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9
2. https://www.restapitutorial.com/httpstatuscodes.html
