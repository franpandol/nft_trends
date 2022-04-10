<template>
<div style="padding: 40px" class="container">
    <div class="search_container">
        <form v-on:submit="search">
            <h3>Search ETH address</h3>
            <input type="text" v-model="message" >
            <input type="submit">
        </form>
    </div>

    <h3>Number of NFTS: {{ nfts_length }}</h3>
    <h3>Owner: {{ owner_address }}</h3>
    
    <NFTCard
        v-for="nft in nfts"
        :key="nft.id"
        :title="nft.title"
        :description="nft.description"
        :image="nft.media[0].gateway"
    />
</div>
</template>

<script>
import axios from "axios";
import NFTCard from "./components/NFTCard.vue";

export default {
    components: {
        NFTCard
    },
    data() {
        return {
            nfts: [],
            nfts_length: null,
            owner_address: null,
        };
    },

    methods: {
        async getData(owner_param) {
            var owner = owner_param;
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
                })
                .catch((error) => console.log(error));
        },

        search(e){
            e.preventDefault() // it prevent from page reload
            this.getData(this.message)
        }
    },

    created() {
    },
};
</script>