# flatmarket

[![Build Status](https://circleci.com/gh/christophercliff/flatmarket.svg?style=shield)](https://circleci.com/gh/christophercliff/flatmarket) [![codecov.io](http://codecov.io/github/christophercliff/flatmarket/coverage.svg?branch=master)](http://codecov.io/github/christophercliff/flatmarket?branch=master)

Flatmarket is a free, open source e-commerce platform for static websites. It is reliable, secure, and inexpensive to operate.

The platform uses [Stripe](https://stripe.com/) for payment processing and is built on the latest web technologies like [hapi](http://hapijs.com/), [React](http://facebook.github.io/react/), and [Webpack](http://webpack.github.io/). It comes with integrations for several cloud platforms including [AWS Lambda](https://aws.amazon.com/lambda/) and [Heroku](https://www.heroku.com/).

At its core is a batteries-included CLI to help you get started quickly. Modules are also [packaged individually](#) so you can customize your rig.

## Demo

:point_right: [christophercliff.com/flatmarket/](https://christophercliff.com/flatmarket/)

You can complete checkout using credit card number `4242 4242 4242 4242`. A test charge will be created in Stripe, so do not submit personal information.

## How it works

Flatmarket is a static website generator paired with a proxy server for sending payments to Stripe. The static website content is generated from a public schema document. The proxy server reads from that schema document during checkout to prevent charge tampering. Once the proxy server is deployed, all content and configuration updates are made by deploying the static website.

### Creating a charge

![architecture](https://cloud.githubusercontent.com/assets/317601/13714569/ff27bb1e-e794-11e5-9861-c04a94f56d35.png)

1. The Web Browser loads the static website from the Static Web Server.
2. The Web Browser submits a charge to Stripe via [Stripe Checkout](https://stripe.com/checkout) and obtains a token.
3. The Web Browser submits the token and product ID to the Flatmarket Service.
4. The Flatmarket Service loads the schema document from the Static Web Server.
5. The Flatmarket Service reads the product information from the schema document and submits the charge to Stripe.

## Features

- Customizable React UI (or use whatever frontend you prefer)
- Separate billing and shipping addresses
- Subscription billing
- Supports many [global currencies](https://support.stripe.com/questions/which-currencies-does-stripe-support)
- Manual [charge authorization](https://support.stripe.com/questions/does-stripe-support-authorize-and-capture)
- Bitcoin
- Mobile-ready

## Documentation

- [Installation](#)
- [Creating the Schema](#)
- [Running Locally](#)
- [Deploying the Static Website](#)
- [Deploying the Proxy Server](#)
- [API Reference](https://github.com/christophercliff/flatmarket/blob/master/REFERENCE.md)
- [Customization](https://github.com/christophercliff/flatmarket/blob/master/CUSTOMIZATION.md)
- [Contributing](https://github.com/christophercliff/flatmarket/blob/master/CONTRIBUTING.md)

### Installation

Install the CLI and [Bananas theme](#):

```sh
$ npm install flatmarket flatmarket-theme-bananas
```

### Creating the Schema

The schema is a JSON document that conforms to the [flatmarket-schema spec](#). It contains information about individual products (e.g. description, price, images), Stripe configuration (e.g. currency, addresses) and any other data necessary to render the static website. It looks [like this](https://github.com/christophercliff/flatmarket-example/blob/master/src/flatmarket.json). By convention, this document should be saved in `src/flatmarket.json`.

### Running Locally

The Flatmarket CLI comes with a local development server so you can preview your website and create charges with your Stripe test keys. The following command will build your webiste and start a development server at [https://127.0.0.1:8000/](https://127.0.0.1:8000/) (note the ***https***).

```sh
$ ./node_modules/.bin/flatmarket ./src/flatmarket.json \
    --component ./node_modules/flatmarket-theme-bananas/index.jsx \
    --stripe-secret-key YOUR_TEST_SECRET_KEY \
    --dev
```

### Deploying the Static Website

When you're finished with development, generate the static website and upload the files to your preferred web server.

```sh
$ ./node_modules/.bin/flatmarket ./src/flatmarket.json \
    --component ./node_modules/flatmarket-theme-bananas/index.jsx
```

### Deploying the Proxy Server

Flatmarket comes with server integrations for the following platforms:

- [AWS Lambda & API Gateway](#)
- [Heroku](#)
- [node.js](#)
- [hapi](#)

Each platform exposes the REST API endpoint that your static website will use to create charges. The server environment requires access to both your Stripe secret key and the public URI for the schema document.

## Platform

- [flatmarket-client](https://github.com/christophercliff/flatmarket-client) A browser client for the REST API endpoint.
- [flatmarket-example](https://github.com/christophercliff/flatmarket-example) An example website.
- [flatmarket-lambda](https://github.com/christophercliff/flatmarket-lambda) TODO
- [flatmarket-schema](https://github.com/christophercliff/flatmarket-schema) A schema specification and validator.
- [flatmarket-server](https://github.com/christophercliff/flatmarket-server) A standalone node.js web server.
- [flatmarket-service](https://github.com/christophercliff/flatmarket-service) TODO
- [flatmarket-server-heroku](https://github.com/christophercliff/flatmarket-server-heroku) A [Heroku](https://www.heroku.com/)-ready web server.
- [hapi-flatmarket](https://github.com/christophercliff/hapi-flatmarket) A hapi plugin.
- [hapi-stripe-webhooks](https://github.com/christophercliff/hapi-stripe-webhooks) A hapi plugin for receiving Stripe notifications.

## Themes

- [Bananas](https://github.com/christophercliff/flatmarket-theme-bananas)

## License

See [LICENSE](https://github.com/christophercliff/flatmarket/blob/master/LICENSE.md).
