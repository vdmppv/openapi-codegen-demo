
# Demo for openapi code generation usage report

## Overview
This server was generated using the [OpenAPI Generator](https://openapi-generator.tech) project.  The code generator, and it's generated code allows you to develop your system with an API-First attitude, where the API contract is the anchor and definer of your project, and your code and business-logic aims to complete and comply to the terms in the API contract.

### prerequisites
- NodeJS >= 10.6
- NPM >= 6.10.0

### Running the server
#### This is a long read, but there's a lot to understand. Please take the time to go through this.
1. Use the OpenAPI Generator to generate your application:
Assuming you have Java (1.8+), and [have the jar](https://github.com/openapitools/openapi-generator#13---download-jar) to generate the application, run:
```java -jar {path_to_jar_file} generate -g nodejs-express-server -i {openapi yaml/json file} -o {target_directory_where_the_app_will_be_installed} ```
If you do not have the jar, or do not want to run Java from your local machine, follow instructions on the [OpenAPITools page](https://github.com/openapitools/openapi-generator). You can run the script online, on docker, and various other ways.
```
npm install @openapitools/openapi-generator-cli -g
```
```
openapi-generator-cli generate -i api/openapi.yaml -g nodejs-express-server
```
2. Go to the generated directory you defined. There's a fully working NodeJS-ExpressJs server waiting for you. This is important - the code is yours to change and update! Look at config.js and see that the settings there are ok with you - the server will run on port 3000, and files will be uploaded to a new directory 'uploaded_files'.
3. The server will base itself on an openapi.yaml file which is located under /api/openapi.yaml. This is not exactly the same file that you used to generate the app:
I.  If you have `application/json` contentBody that was defined inside the path object - the generate will have moved it to the components/schemas section of the openapi document.
II. Every process has a new element added to it - `x-eov-operation-handler: controllers/PetController` which directs the call to that file.
III. We have a Java application that translates the operationId to a method, and a nodeJS script that does the same process to call that method. Both are converting the method to `camelCase`, but might have discrepancy. Please pay attention to the operationID names, and see that they are represented in the `controllers` and `services` directories.
4. Take the time to understand the structure of the application. There might be bugs, and there might be settings and business-logic that does not meet your expectation. Instead of dumping this solution and looking for something else - see if you can make the generated code work for you.
To keep the explanation short (a more detailed explanation will follow): Application starts with a call to index.js (this is where you will plug in the db later). It calls expressServer.js which is where the express.js and openapi-validator kick in. This is an important file. Learn it. All calls to endpoints that were configured in the openapi.yaml document go to `controllers/{name_of_tag_which_the_operation_was_associated_with}.js`, which is a very small method. All the business-logic lies in `controllers/Controller.js`, and from there - to `services/{name_of_tag_which_the_operation_was_associated_with}.js`.

5. Once you've understood what is *going* to happen, launch the app and ensure everything is working as expected:
```
npm start
```
