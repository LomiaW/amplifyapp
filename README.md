#### Getting Started with AWS
### Build a Full-Stack React Application
#### Create a simple web application using AWS Amplify
[Tutorial Link](https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/)

- HOST REACT APP
  - Create a new React Application
    ```
    npx create-react-app amplifyapp
    cd amplifyapp
    npm start
    ```
  - Initialize GitHub repository
    - create a new repo
    - git init and push the new application to repo
      ```
      git init
      git remote add origin git@github.com:user-name/repo-name.git
      git add .
      git commit -m “initial commit”
      git push origin main
      ```
  - Login and deploy the app with AWS Amplify

- INITIALIZE LOCAL APP
  - Install the Amplify Command Line Interface (CLI)
    `npm install -g @aws-amplify/cli`
  - Configure the CLI
    `amplify configure`
  - Initialize the app
    ```
    ? Choose your default editor: Visual Studio Code
    ? Choose the type of app that you're building javascript
    ? What javascript framework are you using react
    ? Source Directory Path:  src 
    ? Distribution Directory Path: build
    ? Build Command:  npm run-script build
    ? Start Command: npm run-script start
    ? Do you plan on modifying this backend? Y
    ```

- ADD AUTHENTICATION
  - Install the Amplify Libraries
    `npm install aws-amplify @aws-amplify/ui-react`
  - Create the authentication service
    ```
    amplify add auth

    ? Do you want to use the default authentication and security configuration? Default configuration
    ? How do you want users to be able to sign in? Username
    ? Do you want to configure advanced settings? No, I am done.
    ```
  - Deploy the authentication service
   `amplify push --y`
  - Configure the React project with Amplify resources
    ```
    import Amplify from 'aws-amplify';
    import config from './aws-exports';
    Amplify.configure(config);
    ```
  - Run the app locally
    `npm start`
  - Set up CI/CD of the front end and backend
    ```
    version: 1
    backend:
      phases:
        build:
          commands:
            - '# Execute Amplify CLI with the helper script'
            - amplifyPush --simple
    frontend:
      phases:
        preBuild:
          commands:
            - npm install
        build:
          commands:
            - npm run build
      artifacts:
        baseDirectory: build
        files:
          - '**/*'
      cache:
        paths:
          - node_modules/**/*
    ```
- ADD API AND DATABASE
  - Create a GraphQL API and database
    `amplify add api`
    ```
    type Todo @model {
      id: ID!
      name: String!
      description: String
    }
    ```
        (Tips: new version aws-amplify/ui-react has no more "Note" "AmplifySignOut" "ListNotes")
  - Write front-end code to interact with the API
    - (see my code in App.js)

- ADD STORAGE
  - Create the storage service
    ```
    amplify add storage
    √ Provide a friendly name for your resource that will be used to label this category in the project: · imagestorage
    √ Provide bucket name: · bucket001
    √ Who should have access: · Auth users only
    √ What kind of access do you want for Authenticated users? · create/update, read, delete
    √ Do you want to add a Lambda Trigger for your S3 Bucket? (y/N) · no
    ✅ Successfully added resource imagestorage locally
    ```
  - Update the GraphQL schema
    ```
    type Todo @model {
      id: ID!
      name: String!
      description: String
      image: String
    }
    ```
  - Deploy storage service and API updates
    `amplify push --y`
  - Update the React app
    - `import { API, Storage } from 'aws-amplify';`
  - Run the app
  - Deleting resources
    ```
    amplify remove auth

    ? Choose the resource you would want to remove: <your-service-name>
    ```
    `amplify push`
    `amplify delete`