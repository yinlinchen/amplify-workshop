# Amplify workshop

## Prerequisite
* Install [Create React App](https://github.com/facebook/create-react-app#creating-an-app) CLI
	```
	npm install -g create-react-app
	```

## Section 1: Creating your first React Application and setup AWS Amplify
* Create a new React project
	```
	create-react-app bookapp
	```
	Or use npx (npm 5.2 & later)
	```
	npx create-react-app bookapp
	```

* Push the project to your GitHub
	```
	cd bookapp
	git remote add origin git@github.com:<github username>/bookapp.git
	git add .
	git commit -m "Initial commit"
	git push origin master
	```

* Install AWS Amplify & AWS Amplify React libraries
	```
	npm install --save aws-amplify aws-amplify-react uuid
	```
* Install the AWS Amplify CLI
	```
	npm install -g @aws-amplify/cli
	```
* Configure the CLI with our credentials
	```
	amplify configure
	```

	```
	Follow these steps to set up access to your AWS account:

	Sign in to your AWS administrator account:
	https://console.aws.amazon.com/
	Press Enter to continue
	
	Specify the AWS Region
	? region:  (Use arrow keys)
	❯ us-west-2 
	  us-east-2 
	
	Specify the username of the new IAM user:
	? user name:  west2
	Complete the user creation using the AWS console
	https://console.aws.amazon.com/iam/home?region=undefined#/.../&policies=arn:aws:iam::aws:policy
	Press Enter to continue

	Enter the access key of the newly created user:
	? accessKeyId:  (<YOUR_ACCESS_KEY_ID>) 
	? secretAccessKey:  (<YOUR_SECRET_ACCESS_KEY>)
	
	This would update/create the AWS Profile in your local machine
	? Profile Name:  amplify-west2
	```

* Initializing A New Project
	```
	amplify init
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
