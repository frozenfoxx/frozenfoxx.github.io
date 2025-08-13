# Cult of Foxx

[![Deploy Quartz site to GitHub Pages](https://github.com/frozenfoxx/frozenfoxx.github.io/actions/workflows/deploy.yaml/badge.svg)](https://github.com/frozenfoxx/frozenfoxx.github.io/actions/workflows/deploy.yaml)

# Requirements
- [Quartz](https://quartz.jzhao.xyz)
- [npm](https://www.npmjs.com/)
- [nvm](https://github.com/nvm-sh/nvm)

# Configuration
- After cloning this repository, initialize all NodeJS packages with running the following:

```shell
npm i
npx quartz create
```

# Usage
- Run `nvm use` to ensure you're using the correct Node version
- To sync updates to the repository, run `npx quartz sync`

# Updates
- To update the major version, update the `.nvmrc`
- To update Quartz, run `npx quartz update`
