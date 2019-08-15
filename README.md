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

* Initializing Amplify project
	```
	amplify init
	```

	```
	? Enter a name for the project bookapp
	? Enter a name for the environment master
	? Choose your default editor: Visual Studio Code
	? Choose the type of app that you're building javascript
	Please tell us about your project
	? What javascript framework are you using react
	? Source Directory Path:  src
	? Distribution Directory Path: build
	? Build Command:  npm run-script build
	? Start Command: npm run-script start
	Using default provider  awscloudformation

	For more information on AWS Profiles, see:
	https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html

	? Do you want to use an AWS profile? Yes
	? Please choose the profile you want to use amplify-west2
	```

* Verify Amplify status
	```
	amplify status
	```

* Start local React application
	```
	npm start
	```

* Deploy your application via the Amplify Console
	```
	git add .
	git commit -m "Add a new function"
	git push origin master
	```

## Section 2: Setup access controls for your application
* Add authentication
	```
	amplify add auth
	```
	```
	Using service: Cognito, provided by: awscloudformation
 
	The current configured provider is Amazon Cognito. 
	 
	Do you want to use the default authentication and security configuration? Default co
	nfiguration
	How do you want users to be able to sign in? Username
	Do you want to configure advanced settings? Yes, I want to make some additional chan
	ges.
	What attributes are required for signing up? Email
	```

* Verify the changes
	```
	amplify status
	```

	```
	Current Environment: master

	| Category | Resource name   | Operation | Provider plugin   |
	| -------- | --------------- | --------- | ----------------- |
	| Auth     | bookappb525b224 | Create    | awscloudformation |
	```

* Push the changes
	```
	amplify push
	```
* Open AWS Cognito console
	```
	amplify console auth
	```
* Configure the React application
	* src/index.js 
	```
	import Amplify from 'aws-amplify'
	import config from './aws-exports'
	Amplify.configure(config)
	```

	* src/App.js 
	```
	import { withAuthenticator } from 'aws-amplify-react'
	import React, { useEffect } from 'react'
	import { Auth } from 'aws-amplify'

	function App() {
	  useEffect(() => {
	    Auth.currentAuthenticatedUser()
	      .then(user => console.log({ user }))
	      .catch(error => console.log({ error }))
	  })
	  return (
	    <div className="App">
	      <p>
	        Edit <code>src/App.js</code> and save to reload.
	      </p>
	    </div>
	  )
	}
	
	export default withAuthenticator(App, { includeGreetings: true })
	```
## Section 3: Introduction to GraphQL and AWS AppSync

## Section 4: Perform data mutations for your application
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

## Section 5: Multiple development environments
* Switch Amplify env
	```
	amplify env checkout developer
	amplify env checkout master
	amplify env list
	```
* Merge branch (Amplify)
	```
	amplify env checkout master
	amplify env pull
	amplify status
	amplify push
	```
* Merge branch (github)
	```
	git checkout master
	git merge developer
	git add .
	git commit -m "add a new function"
	git push origin master
	```

## Project clean up 
* Removing Services
	```
	amplify remove auth
	amplify status
	amplify push
	```

* Deleting entire project
	```
	amplify delete
	```
