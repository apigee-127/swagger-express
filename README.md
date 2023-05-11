# autodesk-forks-swagger-express

This package is a fork of [apigee-127/swagger-express](https://github.com/apigee-127/swagger-express).
The purpose of this fork is to update dependencies and continue to maintain the original package.

Would you like to contribute? Read [our contribution guidelines](./CONTRIBUTING.md).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![NodeJS with Gulp](https://github.com/autodesk-forks/swagger-express/actions/workflows/npm-gulp.yml/badge.svg)](https://github.com/autodesk-forks/swagger-express/actions/workflows/npm-gulp.yml)
![semver](https://img.shields.io/badge/semver-2.0.0-blue)
[![npm version](https://badgen.net/npm/v/autodesk-forks-swagger-express)](https://www.npmjs.com/package/autodesk-forks-swagger-express)
[![contributors](https://img.shields.io/github/contributors/autodesk-forks/swagger-express)](https://github.com/autodesk-forks/swagger-express/graphs/contributors)

## :book: Resources

- [Documentation](./docs/API.md)
- [Typescript definitions](./index.d.ts)
- [Changelog](https://github.com/autodesk-forks/swagger-express/releases)

## Getting started

You can install this fork via npm:
```bash
npm i autodesk-forks-swagger-express
```

The required folder structure (naming can be changed via config parameters):
```
- api
  -- controllers
  -- config
     - default.yaml
  -- swagger
     - swagger.yaml
  -- fittings
```

Sample for `config/default.yaml`:
```yaml
# swagger configuration file

# values in the swagger hash are system configuration for swagger-node
swagger:

  fittingsDirs: [ api/fittings ]
  defaultPipe: null
  swaggerControllerPipe: swagger_controllers  # defines the standard processing pipe for controllers

  # values defined in the bagpipes key are the bagpipes pipes and fittings definitions
  # (see https://github.com/apigee-127/bagpipes)
  bagpipes:

    _router:
      name: swagger_router
      mockMode: false
      mockControllersDirs: [ api/mocks ]
      controllersDirs: [ api/controllers ]

    _swagger_validate:
      name: swagger_validator
      validateResponse: true

    # pipe for all swagger-node controllers
    swagger_controllers:
      - _swagger_validate
      - swagger_security
      - express_compatibility
      - _router

    # pipe to serve swagger file (endpoint is in swagger.yaml)
    swagger_raw:
      name: swagger_raw

# any other values in this file are just loaded into the config for application access...
```

Sample usage with express server:
```javascript
const SwaggerExpress = require("autodesk-forks-swagger-express");
const request = require("supertest");
const express = require('express')();

SwaggerExpress.create({
    appRoot: './test/assets/project',
}, async (err, swaggerExpress) => {
    swaggerExpress.register(express); // now your express server will have all the defined paths and definitions
});```
