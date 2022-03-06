# react-launchpad
A custom React toolchain

# Table of Contents

- [Important](#important)
- [Description](#description)
- [Installation](#installation)
- [Usage](#usage)
- [Dependencies](#dependencies)
- [Credits](#credits)
- [License](#license)

# Important!

### Updating the clone's remote origin

react-launchpad uses the ```git clone``` command to create a clone of the repository to your working directory. A clone in a user's working directory will inherit the repository's remote origin, preventing users from adding the clone to a different remote repository. Users needing to add the clone's contents to their own remote repository will need to replace the clone's inherited origin url with their own remote repository's origin url. Users can find the correct urls when they create a new remote repository on github. 

In the react-launchpad 's root directory, enter the following command into a git bash terminal:

```git remote remove origin```

This command will remove the inherited remote origin from your the of the react-launchpad repository.

To add code from your clone's local repository to a different remote repository on github, navigate to the clone's root directory and enter the one of the following commands into a git bash terminal:

### ssh

```git remote add origin git@github.com:{your-username}/{name-of-remote-repository}.git```

### https
```git remote add origin https://github.com/{your-username}/{name-of-remote-repository}.git```

Be sure to exclude the curly braces around the names of your username and your remote github repository. Running this will update the clone's remote origin to the url that is passed into the command.

# Description
react-launchpad is a simple toolchain for building React JS applications with React-Bootstrap.

react-launchpad acts a foundation for scalable React JS projects & applications. 

It includes a few essential npm dependencies like loaders for Bootstrap's CSS styles, making http requests in React, and writing test suites for your application's code.

Unlike other React toolchains, react-launchpad's design allows developers to include 
additional dependencies, libraries, and babel loaders as needed throughout development.

# Installation

## Create a new react-launchpad app
To create a react-launchpad app, open a new git bash terminal and navigate to the working directory that will store a clone of the repository.

### npx

Inside of the correct working directory enter the following command into your terminal:
```
npx @rlpd/create-react-launchpad-app my-launchpad-app
```

This is the initial react-launchpad app's directory structure:
```
my-launchpad-app
├── dist
|   ├── 45f8382c6f6e1f45f05d3cbd3dbccd1f.png
|   ├── bundle.js
|   └── bundle.LICENSE.txt
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
|   ├── images
|   |   └── launchpad_logo.png
│   └── index.html
└── src
|   ├── __tests__
|   |   ├── App.js
|   |   └── getReact.js
|   ├── util
|   |   └── API.js
|   ├── App.js
|   ├── index.js
|   └── setupTests.js
├── .babelrc
├── .gitignore
├── LICENSE.txt
├── package-lock.json
├── package.json
└── webpack.config.js
```

When the installation successfully completes, the terminal will display the following output:
```
Congrats! Execute the following commands and you're ready to go. Happy coding!
cd my-launchpad-app && npm start
```

Enter ```cd my-launchpad-app && npm start``` into your terminal and your
new react-launchpad app should run on http://localhost:3000/.


Visit http://localhost:3000/ in your browser to see your new react-launchpad app.
<img src="https://i.ibb.co/Ksd8df0/launchpad-app-example.jpg" />

When the app is shut down navigate to your react-launchpad app's root directory and run:
```
npm start
```
To restart your application on http://localhost:3000/

You do not need to install additional dependencies like webpack or babel to run react-launchpad.
These dependencies are already included under devDependencies in the root directory's package.json.

# Usage
### npm run build
To run a production build of your app, you will need to change the webpack configuration's ```mode``` property from ```"development"``` to ```"production"```.

#### webpack.config.js
```
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  mode: "development",  // Change to "production"
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      },
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: 'file-loader',
          },
        ],
      },
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    static: path.join(__dirname, "public"),
    port: 3000,
    open: true,
  },
};
```

Afterwards, you may run ```npm run build``` in a git bash terminal for a production build of the app.

### npm run test
react-launchpad has a setupTests.js file in your ```/src``` directory. This file will import dependencies for testing axios requests and 
React components that use a jest-dom testing enviroment.
It is highly recommended that you do not delete this file, as it contains the necessary imports for axios to mock http requests and
allow jest to run test suites for your application's React components.

#### setupTests.js
```
// These are required imports for axios requests
// and allow jest to use jest-dom as a testing environment
import 'regenerator-runtime/runtime';
import "@testing-library/jest-dom";
```

All tests are located in ```./src/__tests__```. Each react-launchpad app will contain two initial Javascript files.
Each of these Javascript files provide examples for testing React components in a jest-dom environment and mocking axios http requests.

#### App.js
```
/**
 * @jest-environment jsdom
 */
import React from "react";
import { render, screen } from "@testing-library/react";
import App from "../App.js";

it('renders the App comoponent', () => {
    render(<App />);
    expect(screen.getByText('Welcome to Launchpad App!')).toBeInTheDocument();
});
```

Javascript files containing tests for React components must include the following docblock preceding any test code. 
```
/**
 * @jest-environment jsdom
 */
```

jest will setup the proper environment to run DOM rendering tests for your React components.

#### getReact.js
```
import axios from "axios";

jest.mock('axios');

it('makes a request to the react website', async () => {

    expect( await axios.get("https://reactjs.org/"))
        .not.toBeNull();
});
```

jest will mock axios http requests, in this example it expects a response object after making a GET request to <a href="https://reactjs.org/">https://reactjs.org/</a>

At your react-launchpad app's root directory, enter ```npm run test``` to run the test suites in your ```__tests__``` directory.

# Dependencies

react-launchpad currently includes the following dependencies.

- @testing-library/jest-dom: 5.16.2 <a href="https://testing-library.com/docs/ecosystem-jest-dom/">https://testing-library.com/docs/ecosystem-jest-dom/</a>
- @testing-library/react: 12.1.3 <a href="https://www.npmjs.com/package/@testing-library/react">https://www.npmjs.com/package/@testing-library/react</a>
- axios: 0.26.0 <a href="https://axios-http.com/">https://axios-http.com/</a>
- bootstrap: 5.1.3 <a href="https://getbootstrap.com/">https://getbootstrap.com/</a>
- react: 17.0.2 <a href="https://reactjs.org/">https://reactjs.org/</a>
- react-bootstrap: 2.2.0 <a href="https://react-bootstrap.github.io/">https://react-bootstrap.github.io/</a>
- react-dom: 17.0.2 <a href="https://reactjs.org/docs/react-dom.html">https://reactjs.org/docs/react-dom.html</a>

# Credits
react-launchpad is built by Triston Burns.

# License
react-launchpad is open source software licensed as MIT.
