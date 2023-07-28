<template>
  	<div>
		<h1>Transaction Details App</h1>
		<TransactionForm @search="fetchTransactionDetails" />
		<TransactionDetails :transactionHash="searchTransaction"/>
	</div>
</template>

<script>
import TransactionForm from "./components/TransactionForm.vue";
import TransactionDetails from "./components/TransactionDetails.vue";

export default {
	components: {
		TransactionForm,
		TransactionDetails,
	},
	data() {
		return {
			searchTransaction: "",
		};
	},
	methods: {
		fetchTransactionDetails(transactionHash) {
			this.searchTransaction = transactionHash;
		},
	},
};
</script>
