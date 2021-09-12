![Node.js CI (npm)](https://github.com/REPCARDz/repcardz-admin/workflows/Node.js%20CI%20(npm)/badge.svg)

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Up and running

A basic `.env` file is included. After cloning the repo for development, make a file `.env.local` in the project root. Copy over the contents of `.env` into `.env.local`. You can add any other environment variables you want while developing to `.env.local`.

```
export $(cat .env-local | xargs) 
npm i           // install dependencies
npm start       // run the application locally
```

## Deploying

For information on environment set ups, [please see this documentation](https://repcardz.atlassian.net/wiki/spaces/RA/pages/775749641/Environments).

### Dev &amp; QA

Dev: https://repcardz-client-dev-us-east-1.s3.amazonaws.com/index.html
QA: http://client-qa.repcardz.com/

Deployments to Dev and QA are automatically triggered when changes are pushed to the `master` branch, which includes the merging of any pull requests. The status of a build or deployment is available via the [Actions tab](https://github.com/REPCARDz/repcardz-client/actions) on this repo.

There are two repository secrets configured to enable the automatic deployment.
- `AWS_ACCESS_KEY_ID`: The key belongs to the [`repcardz-qa` user](https://console.aws.amazon.com/iam/home?region=us-east-1#/users/repcardz-qa) in AWS. This user does not have console access.
- `AWS_SECRET_ACCESS_KEY`: The key belongs to the `repcardz-qa` user in AWS, along with the key.

### Development &amp; QA

Development: https://repcardz-client-dev-us-east-1.s3.amazonaws.com/index.html
QA: https://client-qa.repcardz.com/index.html

Staging: https://client-staging.repcardz.com/
Production: https://client-prod.repcardz.com/ or https://directory.repcardz.com/

There are four repository secrets configured to enable the automatic deployment. The first two values are not sensitive and are created as `secrets` to they can be updated without code changes.
- `AWS_ACCESS_KEY_ID`: The key belongs to the [`repcardz-qa` user](https://console.aws.amazon.com/iam/home?region=us-east-1#/users/repcardz-qa) in AWS. This user does not have console access.
- `AWS_SECRET_ACCESS_KEY`: The key belongs to the `repcardz-qa` user in AWS, along with the key.

```
REACT_APP_API_URL
REACT_APP_STRIPE_PUBLISHABLE_KEY
```

Then the application is built and the artifacts synced to S3:
```
export $(cat .env-qa | xargs)
npm run build 
aws s3 sync --region us-east-1 build s3://repcardz-client-dev-us-east-1/ // copy the contents of the `build` folder to S3
```

### Staging &amp; Production

Staging: https://client-staging.repcardz.com/index.html
Production: https://client-prod.repcardz.com/index.html (also, https://directory.repcardz.com/index.html)

Staging and production releases are triggered by GitHub tags and follow [semantic versioning](https://semver.org/).

Staging is automatically deployed any time a tag is created matching the pattern of `vX.Y.Z-rc0`. Examples: `v0.0.1-rc1`, `v1.0.0-rc4`. 

Production is automatically deployed any time a tag is created matching the pattern of `vX.Y.Z`. Examples: `v0.0.1`, `v1.0.0`. Staging deploys (tags with the `rc` suffix) are not pushed to production. 
