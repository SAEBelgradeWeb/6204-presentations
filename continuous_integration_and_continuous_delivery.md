---
title: "Continuous Integration and Continuous Delivery"
author: "Vladimir Lelicanin - SAE Institute"
format:
  revealjs:
    theme: default
    title-slide-attributes:
        data-background-image: https://keystoneacademic-res.cloudinary.com/image/upload/f_auto/q_auto/g_auto/w_256/element/15/156456_sae.jpg
        data-background-position: top left
        data-background-repeat: no-repeat
        data-background-size: 100px
    transition: fade
    footer: "Vladimir Lelicanin - SAE Institute Belgrade"
    margin: 0.2
    auto-animate: true
    preview-links: auto
    link-external-newwindow: true
    scrollable: true
    embed-resources: false
    slide-level: 1
    chalkboard: true
    multiplex: true
    slide-number: true
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---

## What is Continuous Integration?
- Continuous Integration (CI) is a practice of integrating code changes into a shared repository frequently
- CI ensures that each code change is verified by an automated build and automated tests

---

### Advantages of Continuous Integration
- Early detection of defects
- Faster feedback loop
- Ability to deliver code changes faster

---

### Continuous Integration with Github Actions
- Github Actions is a CI/CD tool that allows you to automate your software development workflows
- Actions are individual tasks that can be combined to create a workflow

---

### Example of a Github Actions Workflow
```yaml
name: CI

on: [push, pull_request]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'
    - name: Install Dependencies
      run: npm install
    - name: Run Tests
      run: npm test
```

---

#### Explanation of the Workflow
- The workflow is triggered by a push or pull request event
- The workflow runs on an ubuntu-latest runner
- The steps checkout the repository, install dependencies, and run tests

---

### Example of a Successful Action Run
![Image of Github Actions Workflow](https://i0.wp.com/user-images.githubusercontent.com/55514721/101413954-1007d580-389a-11eb-8b15-4f2a45ad9ddb.png?ssl=1)

---

### What is Continuous Delivery?
- Continuous Delivery (CD) is a practice of automating the software delivery process
- CD ensures that your software is always in a deployable state

---

### Continuous Delivery with Github Actions
- Github Actions can be used for Continuous Delivery by deploying to a production environment after the CI tests pass

---

### Example of a Github Actions Workflow for Continuous Delivery
```yaml
name: CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'
    - name: Install Dependencies
      run: npm install
    - name: Build App
      run: npm build
    - name: Deploy to Production
      uses: appleboy/ssh-action@v0.1.5
      with:
        host: ${{ secrets.PROD_SERVER }}
        username: ${{ secrets.SSH_USERNAME }}
        key: ${{ secrets.SSH_KEY }}
        script: |
          cd /var/www/production
          git pull origin main
          npm install --production
          pm2 restart app
```

---

#### Explanation of the Workflow
- The workflow is triggered by a push event to main branch
- The workflow runs on an ubuntu-latest runner
- The steps checkout the repository, install dependencies, build the app, and deploy to production using SSH

---

### Example of a Successful CD Run
![Image of Successful CD Run](https://github.githubassets.com/images/modules/site/actions/pr-checks-final.png)

---

## Github Actions Marketplace
- The Github Actions marketplace contains actions that are created and shared by the community
- These actions can be used as building blocks to create your own workflows

---

### Example Github Actions Marketplace Action
- [Sentry Release](https://github.com/getsentry/action-release)

```yaml
name: Sentry Release

on:
  push:
    branches:
      - master

jobs:
  sentry-release:
    name: Sentry Release
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: getsentry/action-release@v1
        id: sentry_release
        with:
          version: ${{ github.sha }}
        env:
          SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
          SENTRY_ORG: ${{ secrets.SENTRY_ORG }}
          SENTRY_PROJECT: ${{ secrets.SENTRY_PROJECT }}
```
---

### Explanation of Sentry Release Action
- The action is triggered by a push event to the master branch
- The steps checkout the repository and use the Sentry Release action to create a new release on Sentry

---

### Links to Documentation
- [Github Actions Documentation](https://docs.github.com/en/actions)
- [Continuous Integration Best Practices](https://www.thoughtworks.com/continuous-integration)
- [Continuous Delivery Best Practices](https://www.thoughtworks.com/continuous-delivery)

---

### Conclusion
- Continuous Integration and Continuous Delivery are critical components of modern software development
- Github Actions provides an easy way to automate your development workflows

---

## References
- [Github Actions](https://github.com/features/actions)
- [Sentry Release Action](https://github.com/getsentry/action-release)
- [Node.js](https://nodejs.org/en/)
- [PM2](https://pm2.keymetrics.io/)
