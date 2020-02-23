# Amplify workshop

## Prerequisite
* Install [Create React App](https://github.com/facebook/create-react-app#creating-an-app) CLI
	```
	npm install -g create-react-app
	```

## Presentation 
* [Slides](Amplify-workshop.pdf)
* [AWS EC2 instruction](aws/readme.md)

## Section 1: Creating your first React Application and setup AWS Amplify
* Install the AWS Amplify CLI
	```
	npm install -g @aws-amplify/cli
	```

* Create a new React project
	```
	create-react-app bookapp
	```
	Or use npx (npm 5.2 & later)
	```
	npx create-react-app bookapp
	```

* Change react app listening port
	```
	"start": "react-scripts start"
	or
	"start": "PORT=8080 react-scripts start"
	```

* Push the project to your GitHub (Optional)
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

	Alternative way. Enter the access key manually
	```
	Specify the AWS Region
	? region:  us-west-2
	Specify the username of the new IAM user:
	? user name:  west-2
	Complete the user creation using the AWS console
	https://console.aws.com/xxxx
	Press Enter to continue

	Enter the access key of the newly created user:
	? accessKeyId:  
	? secretAccessKey:  
	This would update/create the AWS Profile in your local machine
	? Profile Name:  west-2
	```

* Verify Amplify status
	```
	amplify status
	```

* Start local React application
	```
	npm start
	```

* Deploy your application via the Amplify Console (Optional)
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
	Do you want to configure advanced settings? No, I am done.
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

* Open AWS Cognito console (Optional)
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
	        Hello! This is an AWS Amplify application.
	      </p>
	    </div>
	  )
	}

	export default withAuthenticator(App, { includeGreetings: true })
	```

* Add E2E tests to app with Cypress
	* Install Cypress
	```
	yarn add cypress --dev
	```
	* Add a test spec file
	```
	cd cypress/integration
	rm -rf examples
	touch authenticator_spec.js
	```
	* cypress/integration/authenticator_spec.js
	```
	describe('Authenticator:', function() {
	  // Step 1: setup the application state
	  beforeEach(function() {
	    cy.visit('/');
	  });

	  describe('Sign In:', () => {
	    it('allows a user to signin', () => {
	      // Step 2: Take an action (Sign in)
	      cy.get(selectors.usernameInput).type("code4lib");
	      cy.get(selectors.signInPasswordInput).type("workshop");
	      cy.get(selectors.signInSignInButton).contains('Sign In').click();

	      // Step 3: Make an assertion (Check for sign-out text)
	        cy.get(selectors.signOutButton).contains('Sign Out');
	    });
	  });

	});
	export const selectors = {
	  // Auth component classes
	  usernameInput: '[data-test="username-input"]',
	  signInPasswordInput: '[data-test="sign-in-password-input"]',
	  signInSignInButton: '[data-test="sign-in-sign-in-button"]',
	  signOutButton: '[data-test="sign-out-button"]'
	}
	```
	* cypress.json
	```
	{ "baseUrl": "http://localhost:3000/" }
	```

## Section 3: Introduction to GraphQL and AWS AppSync
* Adding a GraphQL API
	```
	amplify add api

	? Please select from one of the below mentioned services: GraphQL
	? Provide API name: BookGraphQL
	? Choose the default authorization type for the API API key
	? Enter a description for the API key: 
	? After how many days from now the API key should expire (1-365): 7
	? Do you want to configure advanced settings for the GraphQL API No, I am done.
	? Do you have an annotated GraphQL schema? No
	? Do you want a guided schema creation? Yes
	? What best describes your project: Single object with fields (e.g., “Todo” with ID, name, description)
	? Do you want to edit the schema now? Yes
	```

* Edit the schema
	```
	type Book @model {
	  id: ID!
	  bookId: ID
	  name: String!
	  category: String!
	  description: String
	  price: Float!
	}
	```

* Submit GraphQL schema
	```
	amplify status
	```

	```
	Current Environment: master

	| Category | Resource name   | Operation | Provider plugin   |
	| -------- | --------------- | --------- | ----------------- |
	| Api      | BookGraphQL     | Create    | awscloudformation |
	| Auth     | bookappb525b224 | No Change | awscloudformation |
	```

	```
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

## Section 4: Perform data mutations for your application
* Create a book record
	```
	mutation createBook {
	  createBook(input: {
	    name: "AWS Amplify book"
	    category: "programming"
	    description: "A book about AWS Amplify"
      price: 90
	  }) {
	    id name category description price
	  }
	}
	```

* Perform query
	```
	query listBooks {
	  listBooks {
	    items {
	      id
	      name
	      category
	      description
	      price
	   }
	  }
	}
	```

* Perform filter query
	```
	query listBooksbyPrice {
	  listBooks(filter: {
	    price: {
	      gt: 20
	    }
	  }) {
	    items {
	      id
	      name
	      category
	      description
	      price
	    }
	  }
	}
	```

* Configure the React application
	* src/App.js
	```
	import React, { useEffect, useState }  from 'react'
	import { Auth, API, graphqlOperation } from 'aws-amplify'
	import './App.css';

	import { withAuthenticator } from 'aws-amplify-react'

	// import query
	import { listBooks } from './graphql/queries'


	function App() {
	  useEffect(() => {
	    Auth.currentAuthenticatedUser()
	      .then(user => console.log({ user }))
	      .catch(error => console.log({ error }))
	  })

	  const [books, updateBooks] = useState([])

	  useEffect(() => {
	    getData()
	  }, [])

	  async function getData() {
	    try {
	      const bookData = await API.graphql(graphqlOperation(listBooks))
	      console.log('data from API: ', bookData)
	      updateBooks(bookData.data.listBooks.items)
	    } catch (err) {
	      console.log('error fetching data..', err)
	    }
	  }

	  return (
	    <div className="App">
	      <p>
	          Hello! This is an AWS Amplify application.
	      </p>
	      {
	        books.map((c, i) => (
	          <div key={i}>
	            <h2>Title: {c.name}</h2>
	            <h4>Category: {c.category}</h4>
	            <h4>Description: {c.description}</h4>
	            <p>Price: {c.price}</p>
	          </div>
	        ))
	      }
	    </div>
	  );
	}

	export default withAuthenticator(App, { includeGreetings: true })
	```

* Performing mutations
	```
	import React, { useEffect, useReducer }  from 'react'
	import { Auth, API, graphqlOperation } from 'aws-amplify'
	import './App.css';

	import { withAuthenticator } from 'aws-amplify-react'

	// import query
	import { listBooks } from './graphql/queries'

	import { createBook as CreateBook } from './graphql/mutations'

	import uuid from 'uuid/v4'

	const BOOK_ID = uuid()

	// create initial state
	const initialState = {
	  name: '', price: '', category: '', description: '', books: []
	}

	// create reducer to update state
	function reducer(state, action) {
	  switch(action.type) {
	    case 'SETBOOKS':
	      return { ...state, books: action.books }
	    case 'SETINPUT':
	      return { ...state, [action.key]: action.value }
	    default:
	      return state
	  }
	}

	function App() {
	  useEffect(() => {
	    Auth.currentAuthenticatedUser()
	      .then(user => console.log({ user }))
	      .catch(error => console.log({ error }))
	  })

	  const [state, dispatch] = useReducer(reducer, initialState)

	  useEffect(() => {
	    getData()
	  }, [])

	  async function getData() {
	    try {
	      const bookData = await API.graphql(graphqlOperation(listBooks))
	      console.log('data from API: ', bookData)
	      dispatch({ type: 'SETBOOKS', books: bookData.data.listBooks.items})
	    } catch (err) {
	      console.log('error fetching data..', err)
	    }
	  }

	  async function createBook() {
	    const { name, price, category, description } = state
	    if (name === '' || price === '' || category === '' || description === '') return
	    const book = {
	      name, price: parseFloat(price), category, description, bookId: BOOK_ID
	    }
	    const books = [...state.books, book]
	    dispatch({ type: 'SETBOOKS', books })
	    console.log('book:', book)
	    
	    try {
	      await API.graphql(graphqlOperation(CreateBook, { input: book }))
	      console.log('item created!')
	    } catch (err) {
	      console.log('error creating book...', err)
	    }
	  }

	  // change state then user types into input
	  function onChange(e) {
	    dispatch({ type: 'SETINPUT', key: e.target.name, value: e.target.value })
	  }

	  return (
	    <div className="App">
	      <br/><br/>
	      <input
	        name='name'
	        placeholder='name'
	        onChange={onChange}
	        value={state.name}
	      />
	      <input
	        name='price'
	        placeholder='price'
	        onChange={onChange}
	        value={state.price}
	      />
	      <input
	        name='category'
	        placeholder='category'
	        onChange={onChange}
	        value={state.category}
	      />
	      <input
	        name='description'
	        placeholder='description'
	        onChange={onChange}
	        value={state.description}
	      />
	      <button onClick={createBook}>Create Book</button>

	      <br/>
	      <br/>
	      {
	        state.books.map((c, i) => (
	          <div key={i}>
	            <h2>Title: {c.name}</h2>
	            <h4>Category: {c.category}</h4>
	            <h4>Description: {c.description}</h4>
	            <p>Price: {c.price}</p>
	          </div>
	        ))
	      }
	    </div>
	  );
	}

	export default withAuthenticator(App, { includeGreetings: true })
	```

* Add a REST API
	```
	? Please select from one of the below mentioned services REST
	? Provide a friendly name for your resource to be used as a label for this category in the project: amplifyrestapi
	? Provide a path (e.g., /items) /items
	? Choose a Lambda source Create a new Lambda function
	? Provide a friendly name for your resource to be used as a label for this category in the project: amplifyrestapifunction
	? Provide the AWS Lambda function name: amplifyrestapifunction
	? Choose the function template that you want to use: Serverless express function (Integration with Amazon API Gateway)
	? Do you want to access other resources created in this project from your Lambda function? No
	? Do you want to edit the local lambda function now? Yes
	Please edit the file in your editor: /<your file path>/amplify/backend/function/amplifyrestapifunction/src/index.js
	? Press enter to continue 
	Succesfully added the Lambda function locally
	? Restrict API access Yes
	? Who should have access? Authenticated users only
	? What kind of access do you want for Authenticated users? (Press <space> to select, <a> to toggle all, <i> to invert selection)
	? Do you want to add another path? No
	```

* Interacting with the new API
	```
	let [message, setMessage] = useState('')

	async function getData() { 
	    let apiName = 'test';
	    let path = '/items?q=test';
	    let myInit = { // OPTIONAL
	        headers: {} // OPTIONAL
	    }
	    const apiResponse = await API.get(apiName, path, myInit);
	    setMessage(apiResponse.query)
	    return apiResponse.query
	}

	This is the message from REST API: {message}
  	<br/>
  	<button onClick={() => getData()}>
    	Show message
  	</button>
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
