# react-library-scaffold

React has become one of the most popular front-end frameworks due to its component-based architecture, and with the ease of sharing packages through Npm, building and sharing your own React components has never been easier.
In this guide, we'll walk you through the steps of building your own React component from scratch, using the Vite-React framework to streamline the process. We'll cover everything from setting up your development environment to publishing your component on NPM, ensuring that it is both functional and accessible for other developers to use.

## Prerequisites

Here are some prerequisites you need to have before diving into building a React component with Vite and publishing it on npm:

- Basic knowledge of React, JavaScript, and NPM.

- Node.js installed on your machine.

- An N.P.M account to publish your package.

- A GitHub Account for Hosting your Hosting your code repository


## Getting Started

To get started, we'll create a new React project using Vite. Vite is a build tool that is optimized for frontend development, and it provides a fast and easy way to set up a new React project.
First, let's create a new directory for our project and navigate to it in the terminal.

- First Step is to initialize the Vite-React package in our project directory by running the following command:

```
yarn create Vite react-library-scaffold 
```
follow the prompt using the arrow key to navigate to react and javascript

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b69l9rhorqk7888e2rzo.png)

-  Next navigate to the newly created Folder and run the yarn to install the react dependencies

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bqpfrynlrad3s851swfb.png)

- On completion your file structure should look like this <br/>
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/far6wjdswqkw3xk1qvwa.PNG)

-  Next step is to add more information to the package.Json file and modify our vite.config.js files  
open your terminal and type `npm init` and enter the name of the library, descriptions, authors ... the new code block should look like this.

```
{
  "name": "react-library-scafold",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint src --ext js,jsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.28",
    "@types/react-dom": "^18.0.11",
    "@vitejs/plugin-react": "^4.0.0",
    "eslint": "^8.38.0",
    "eslint-plugin-react": "^7.32.2",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.3.4",
    "vite": "^4.3.2"
  },
  "description": "A react-Library-Scaffold",
  "main": "vite.config.js",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/Petsamuel/react-library-scaffold.git"
  },
  "keywords": [
    "react",
    "components"
  ],
  "author": "Peter Samuel",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/Petsamuel/react-library-scaffold/issues"
  },
  "homepage": "https://github.com/Petsamuel/react-library-scaffold#readme"
}
```
Add this Block code to your Package.json
it's basically specifying where the build folder needed for the component should be stored. 
```
 "main": "./dist/react-library-scaffold.umd.js",
  "module": "./dist/react-library-scaffold.es.js",
  "exports": {
    ".": {
      "import": "./dist/react-library-scaffold.es.js",
      "require": "./dist/react-library-scaffold.umd.js"
    }
  },
```

- Next step is to install test dependencies vite, react-test-renderer

```
npm install @testing-library/dom @testing-library/react c8 eslint eslint-plugin-react jsdom react-test-renderer vitest --save-dev

```
then add the test script to your package.json file
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ygdlj3ictugceq7sdo3a.PNG)

- Next step is configuring the Vite.config.jsx file

add this block of code there
```
import path from 'path'
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  build: {
    lib: {
      entry: path.resolve("src", 'src/components/index.jsx'),
      name: 'react-library-scaffold',
      fileName: (format) => `react-library-scaffold.${format}.js`
    },
    rollupOptions: {
      external: ['react', 'react-dom'],
      output: {
        globals: {
          react: 'React'
        }
      }
    }
  },
  plugins: [react()]
})
```
Make sure you replace name: `'react-library-scaffold'` with the name of your component

Likewise, make sure that `react-library-scaffold` in the filename value is changed to the name of your component

## Configure Testing
create a vitest.config.js file in your root folder and add the following code

```
import { defineConfig } from 'vite'

export default defineConfig({
  test: {
    globals: false,
    environment: 'jsdom'
  }
})
```

## Creating Components along with Tests Snapshots

The entrypoint for your component is /src/components/index.jsx. 

For now you we want to create a dummy HelloWorld component as in the examples below, then once the development environment is set up, come back and build your component.

- Create your component in /src/component/index.jsx. Here's a very simple example:
```
import React from 'react'

export default function Greeting(props) {
  const {
    message= 'World'
  } = props

  return (
    <div>Hello, {greeting}!</div>
  )
}
```

- Add test

```
import React from 'react'
import renderer from 'react-test-renderer'
import { describe, expect, it} from 'vitest'
import Greeting from './index'

describe('Greeting component', () => {
  it('Greeting component renders correctly', () => {
    const component = renderer.create(
      <Greeting/>
    )

    const tree = component.toJSON()

    expect(tree).toMatchSnapshot()
  })

  it(' prop working', () => {
    const component = renderer.create(
      <Greeting message={'Universe'} />
    )

    const tree = component.toJSON()

    expect(tree).toMatchSnapshot()
  })
})
```
You can run your tests by typing:
```
npm test
or Yarn test
```
Or you can run tests and set a watch to re-run tests when anything changes.

```
 npm run watch
or yarn watch
```
now open the main.jsx and create a component 
sample code
```
import React from 'react'
import Greeting from './components'

const App = () => {
  return (
    <Greeting message={'Universe'} />
  )
}

export default App
```
now run the development server using 
`yarn dev`



## Writing a Customized Readme 
To make it easy for other developers to use your component, it's crucial to create a high-quality README file that contains comprehensive documentation, usage examples, and highlights the features and advantages of using your component.

Open /src/README.md

Create your README file using markdown.

## Pushing to a GitHub repository:

- Create a new repository on GitHub.
- Initialize a Git repository in your project folder `using git init.`
- Add all files to the repository using `git add .`.
- Commit your changes using `git commit -m "Initial commit"`.
- Set the remote origin to your GitHub repository `using git remote add origin <repository-url>`.
- Push your code to the repository using `git push -u origin main`.

## Publishing to npm
We are finally ready to publish our component to npm.

To ensure a smooth publishing process, it is important to follow this checklist:

Verify that the version number in your package.json file is accurate and adheres to the semantic versioning convention. Each npm publication should have a unique version number.

Ensure all tests pass:

```
npm test
```
Create the build files:

```
npm run build
```
Both UMD and ESM module formats are created and placed in the /dist folder. Again, note that React is not bundled in alongside your component. Just your component code and any dependencies.

Ensure you are logged into npm. If not type:

```
 yarn login
```
Publish your component

```
 yarn publish
```
And all done!
All the code in this guide is available in the [repository on Github](https://github.com/Petsamuel/react-library-scaffold.git).

