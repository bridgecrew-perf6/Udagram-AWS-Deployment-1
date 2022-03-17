# Pipeline Process

## Introduction

This project is deployed using Circleci, the configuration file is in `./circleci/config.yml`.
## Pipeline Description
#### Specify the Circleci version, version 2.1 is used
```
version: 2.1
```
#### Configure the pipeline using the following orbs to setup the software on the server executing the pipeline: `node`, `aws-cli`, `eb`, `browser-tools`.
```
node: circleci/node@5.0.0  
aws-cli: circleci/aws-cli@2.1.0  
eb: circleci/aws-elastic-beanstalk@2.0.1  
browser-tools: circleci/browser-tools@1.2.3
```
#### Jobs are then executed in the following order:
- Install frontend and backend packages and dependencies
- Test frontends
- Build front and backend
- Deploy backend and frontend
```
jobs:  
  build:  
    docker:  
      - image: 'cimg/base:stable'  
  steps:  
      - node/install:  
          node-version: '14.17'  
  - run: node --version  
      - checkout  
      - aws-cli/setup  
      - eb/setup  
      - browser-tools/install-chrome  
      - browser-tools/install-chromedriver  
      - run:  
          name: Install Frontend Packages  
          command: |  
            npm run frontend:install  
      - run:  
          name: Install Backend Packages  
          command: |  
            npm run backend:install  
      - run:  
          name: Test Frontend  
          command: |  
            npm run frontend:test  
      - run:  
          name: Build Frontend  
          command: |  
            npm run frontend:build  
      - run:  
          name: Build Backend  
          command: |  
            npm run backend:build  
      - run:  
          name: Deploy Backend  
          command: |  
            npm run backend:deploy  
      - run:  
          name: Deploy Frontend  
          command: |  
            npm run frontend:deploy
```

#### The above command scripts are defined in `package.json` at the root folder. Scripts navigate to the frontend or backend folder and execute the corresponding script defined in their own `package.json`.
```
"frontend:install": "cd udagram-frontend && npm install",  
"backend:install": "cd udagram-api && npm install",  
"frontend:test": "cd udagram-frontend && npm run test:ci",  
"frontend:build": "cd udagram-frontend && npm run build",  
"backend:build": "cd udagram-api && npm run build",  
"frontend:deploy": "cd udagram-frontend && npm run deploy",  
"backend:deploy": "cd udagram-api && npm run deploy"
```