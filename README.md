# nft_trends
A vue.js app to list all NFTs owned by some address using Alchemy & axios

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Steps to use this app

1: You need an ALCHEMY API KEY
https://docs.alchemy.com/alchemy/introduction/getting-started#1.create-an-alchemy-key

After that, create a .env file and put the KEY in there
`VUE_APP_ALCHEMY_API_KEY=<YOUR_API_KEY>`


2: In `App.vue` replace this line with your own address
```
var owner = "<Ethereum address>";
```