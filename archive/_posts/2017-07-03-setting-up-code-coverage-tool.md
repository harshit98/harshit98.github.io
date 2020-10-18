---
layout: post
title: "Setting up Code Coverage Tool"
date: 2017-07-03
category: archive
comments: true
author: "Harshit Prasad"
tags: [code-coverage, testing, codecov, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

In this blog post, I’ll be discussing how I setup code coverage tool called ***Codecov*** in Susper.

#### What is Codecov ?
Codecov is a famous code coverage tool. It can be easily integrated with the services like Travis CI. Codecov also provides more features with the services like Docker.

#### How did I setup Codecov in the project repository hosted on Github ?
The simplest way to setup Codecov in a project repository is by installing `codecov.io` using the terminal command:

```bash
npm install --save-dev codecov.io
```

Susper works on tech-stack Angular. Angular comes with Karma and Jasmine for testing purpose. With Angular, implementation can be a little bit tricky. So, using alone:

```bash
bash <(curl -s https://codecov.io/bash)
```

won’t generate code coverage because of the presence of Karma and Jasmine.

It will require additional two packages: 

- `istanbul` as coverage reporter and 
- `jasmine` as html reporter.

I have discussed them below.

Install these two packages:

#### karma-coverage-istanbul-reporter

```bash
npm install karma-coverage-istanbul-reporter --save-dev
```

#### karma-jasmine html-reporter

```bash
npm install karma-jasmine-html-reporter --save-dev
```

After installing the codecov.io, the `package.json` will be updated as follows:
```json
"devDependencies": {
  "codecov": "^2.2.0",
  "karma-coverage-istanbul-reporter": "^1.3.0",
  "karma-jasmine-html-reporter": "^0.2.2",
}
```

Add a script for testing:
```json
"scripts": {
   "test": "ng test --single-run --code-coverage --reporters=coverage-istanbul"
}
```

Now generally, the codecov works better with Travis CI. With the one line bash <(curl -s https://codecov.io/bash) the code coverage can now be easily reported.

Here is a particular example of `travis.yml` from the project repository of Susper:
```yaml
script:
 - ng test --single-run --code-coverage --reporters=coverage-istanbul
 - ng lint
 
after_success:
 - bash <(curl -s https://codecov.io/bash)
 - bash ./deploy.sh
```

Update `karma.config.js` as well:

```js
Module.exports = function(config) {
  config.set({
    plugins: [
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter')
    ],
    preprocessors: {
      'src/app/**/*.js': ['coverage']
    },
    client: {
      clearContext: false
    },
    coverageIstanbulReporter: {
      reports: ['html', 'lcovonly'],
      fixWebpackSourcePaths: true
    },
    reporters: config.angularCli && config.angularCli.codeCoverage
      ? ['progress', 'coverage-istanbul'],
      : ['progress', 'kjhtml'],
  })
}
```

In this way, we can setup any code coverage tool like Codecov in an Angular project follow up the approach shared as above.
