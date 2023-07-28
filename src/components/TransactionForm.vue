<template>
    <div>
        <input v-model="transactionHash" placeholder="Enter Transaction Hash" />
        <button @click="searchTransaction">Search</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            transactionHash: "",
        };
    },
    methods: {
        searchTransaction() {
            if (this.transactionHash.trim() !== "") {
                this.$emit("search", this.transactionHash.trim());
            }
        },
    },
};
</script>