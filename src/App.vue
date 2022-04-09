<template>
Number of NFTS: {{ nfts_length }}<br />
Owner: {{ owner_address }}
<div style="padding: 40px" class="col-12" v-for="nft in nfts" v-bind:key="nft.id">
    <h3>{{ nft.title }}</h3>
    <p>{{ nft.description }}</p>
    <img :src="nft.metadata.image" />
</div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            nfts: [],
            nfts_length: null,
            owner_address: null,
        };
    },

    methods: {
        async getData() {
            // replace with your own address
            var owner = "<Ethereum address>";
            // construct the axios request:
            var config = {
                method: "get",
                url: "https://eth-mainnet.alchemyapi.io/v2/" +
                    process.env.VUE_APP_ALCHEMY_API_KEY +
                    "/getNFTs/?owner=" +
                    owner,
            };
            // make the request and print the formatted response:
            axios(config)
                .then((response) => {
                    this.nfts = response.data.ownedNfts;
                    this.nfts_length = this.nfts.length;
                    this.owner_address = owner;

					for (const nft in this.nfts) {
						// replace ipfs with a public ipfs node to see the images
                        this.nfts[nft].metadata.image =
                            "https://gateway.pinata.cloud/ipfs/" +
                            this.nfts[nft].metadata.image.replace("ipfs://", "");
                    }
                })
                .catch((error) => console.log(error));
        },
    },

    created() {
        this.getData();
    },
};
</script>