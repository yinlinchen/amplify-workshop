# Amplify workshop

## Prerequisite
* Install [Create React App](https://github.com/facebook/create-react-app#creating-an-app) CLI
```
npm install -g create-react-app
```

## Section 1: Creating your first React Application and setup AWS Amplify
* Create a new React project & change into the new directory using the Create React App CLI.
```

create-react-app bookapp

```



## GraphQL API
* Adding a GraphQL API
```
amplify add api

Please select from one of the above mentioned services GraphQL
Provide API name: CryptoGraphQL
Choose an authorization type for the API API key
Do you have an annotated GraphQL schema? N
Do you want a guided schema creation? Y
What best describes your project: Single object with fields (e.g. “Todo” with ID, name, description)
Do you want to edit the schema now? (Y/n) Y
```
* Edit the schema
```
type Book @model {
  id: ID!
  clientId: ID
  name: String!
  category: String!
  price: Float!
}
```
* Submit GraphQL schema
```
amplify status
amplify push

Do you want to generate code for your newly created GraphQL API Y
Choose the code generation language target: javascript
Enter the file name pattern of graphql queries, mutations and subscriptions: (src/graphql/**/*.js)
Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions? Y
Enter maximum statement depth [increase from default if your schema is deeply nested] 2
```
* Open AppSync Console
```
amplify console api

Please select from one of the below mentioned services GraphQL
```
