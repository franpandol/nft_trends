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

var alchemyBaseUrl = "https://eth-mainnet.alchemyapi.io/v2/" + process.env.VUE_APP_ALCHEMY_API_KEY;
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
        async getNFTData(owner_param) {
            var owner = owner_param;
            var config = {
                method: "get",
                url: alchemyBaseUrl +
                    "/getNFTs/?owner=" +
                    owner,
            };


            axios(config)
                .then((response) => {
                    this.nfts = response.data.ownedNfts;
                    this.nfts_length = this.nfts.length;
                    this.owner_address = owner;
                })
                .catch((error) => console.log(error));

        },
        async getAssetsTransfers(owner_param){
            let data = JSON.stringify({
                "jsonrpc": "2.0",
                "id": 0,
                "method": "alchemy_getAssetTransfers",
                "params": [
                    {
                    "fromBlock": "0x0",
                    "fromAddress": owner_param,
                    }
                ]
                });

            var requestOptions = {
                method: 'post',
                headers: { 'Content-Type': 'application/json' },
                data: data,
            };

            axios(alchemyBaseUrl, requestOptions)
                .then(response => console.log(JSON.stringify(response.data, null, 2)))
                .catch(error => console.log(error));

        },

        search(e){
            e.preventDefault() // it prevent from page reload
            this.getNFTData(this.message)
            this.getAssetsTransfers(this.message)
        }
    },

    created() {
    },
};
</script>