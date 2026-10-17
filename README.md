# ember-cli-deploy-aws-pack

> [!WARNING]
> **This package is deprecated and no longer maintained.** It only bundled five ember-cli-deploy plugins and a config blueprint, so you can get the same setup by installing those plugins directly. See [Migrating away](#migrating-away) below.

## Migrating away

Your existing `config/deploy.js` works unchanged, because the plugins read the same configuration whether they come from this pack or are installed directly.

1. Install the plugins directly:

   ```
   ember install ember-cli-deploy-build ember-cli-deploy-gzip ember-cli-deploy-manifest ember-cli-deploy-s3 ember-cli-deploy-cloudfront
   ```

2. Remove the pack:

   ```
   npm uninstall ember-cli-deploy-aws-pack
   ```

3. Run a deploy to confirm the pipeline still works.

### Starting a new project

If you want this pack's starting config for a new app, install `ember-cli-deploy` and the plugins above, then use this as your `config/deploy.js`:

```js
'use strict';

module.exports = function (deployTarget) {
  let ENV = {
    build: {},
    pipeline: {
      // Runs the activation hooks on every deploy, which ember-cli-deploy-cloudfront needs
      activateOnDeploy: true,
    },
    s3: {
      accessKeyId: process.env.AWS_KEY,
      secretAccessKey: process.env.AWS_SECRET,
      filePattern: '*',
    },
    cloudfront: {
      accessKeyId: process.env.AWS_KEY,
      secretAccessKey: process.env.AWS_SECRET,
    },
  };

  if (deployTarget === 'staging') {
    ENV.build.environment = 'production';
    ENV.s3.bucket = process.env.STAGING_BUCKET;
    ENV.s3.region = process.env.STAGING_REGION;
    ENV.cloudfront.distribution = process.env.STAGING_DISTRIBUTION;
  }

  if (deployTarget === 'production') {
    ENV.build.environment = 'production';
    ENV.s3.bucket = process.env.PRODUCTION_BUCKET;
    ENV.s3.region = process.env.PRODUCTION_REGION;
    ENV.cloudfront.distribution = process.env.PRODUCTION_DISTRIBUTION;
  }

  return ENV;
};
```

---

_The original README follows, for reference._


[![Ember CI](https://github.com/kpfefferle/ember-cli-deploy-aws-pack/workflows/Ember%20CI/badge.svg)](https://github.com/kpfefferle/ember-cli-deploy-aws-pack/actions)


> An ember-cli-deploy plugin pack to implement a simple AWS deployment pattern

![](https://ember-cli-deploy.github.io/ember-cli-deploy-version-badges/plugins/ember-cli-deploy-aws-pack.svg)

This package bundles the plugins you need to have a deployment pipeline for your Ember app similar to what I described in my blog post: [Deploying Ember to AWS CloudFront](http://blog.testdouble.com/posts/2015-11-03-deploying-ember-to-aws-cloudfront-using-ember-cli-deploy.html)

It also has a blueprint for your `config/deploy.js` file to get you started.

## Installation

```
ember install ember-cli-deploy
ember install ember-cli-deploy-aws-pack
```

The necessary set of plugins will be available to ember-cli-deploy and an example `deploy/config.js` file will be generated for you to customize with information for your deployment environments.

## What is a plugin pack?

A "plugin pack" is a concept supported by ember-cli-deploy that allows a single addon to make multiple plugins available by adding a single direct dependency to your project.

## What plugins are made available?

* [ember-cli-deploy-build](https://github.com/ember-cli-deploy/ember-cli-deploy-build)
* [ember-cli-deploy-cloudfront](https://github.com/kpfefferle/ember-cli-deploy-cloudfront)
* [ember-cli-deploy-gzip](https://github.com/ember-cli-deploy/ember-cli-deploy-gzip)
* [ember-cli-deploy-s3](https://github.com/ember-cli-deploy/ember-cli-deploy-s3)
* [ember-cli-deploy-manifest](https://github.com/ember-cli-deploy/ember-cli-deploy-manifest)
