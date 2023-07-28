<template>
	<div class="transaction-details" v-if="transactionData">
	    <h2>Transaction Details</h2>
	    <div class="detail-row">
	      <span class="detail-label">Transaction Hash:</span>
	      <span class="detail-value">{{ transactionHash }}</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Sender Address:</span>
	      <span class="detail-value">{{ transactionData?.ownerAddress }}</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Receiver Address:</span>
	      <span class="detail-value">{{ transactionData?.toAddress }}</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Amount:</span>
	      <span class="detail-value">{{ transactionData.contractData.amount }} TRX</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Gas Price:</span>
	      <span class="detail-value">{{ transactionData?.gasPrice }} TRX</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Status:</span>
	      <span class="detail-value">{{ transactionData.confirmed ? 'Confirmed' : 'Not Confirmed' }}</span>
	    </div>
	    <div class="detail-row">
	      <span class="detail-label">Request Origin:</span>
	      <span class="detail-value">Online</span>
	    </div>
	</div>
	<div v-if="!transactionData">
		Note: Valid Transaction Hash Only
	</div>
</template>

<script>
import axios from "axios";

export default {
    props: {
        transactionHash: {
            type: String,
            required: true,
        },
    },
    data() {
        return {
            transactionData: null
        };
    },
    watch: {
        transactionHash() {
            this.loadTransactionDetails();
        },
    },
    created() {
        // Load transaction details from API or local storage
        this.loadTransactionDetails();
    },
    methods: {
        loadTransactionDetails() {
            // Check if transaction details are in local storage
            const cachedTransactionData = localStorage.getItem(this.transactionHash);
            if (cachedTransactionData) {
                this.transactionData = JSON.parse(cachedTransactionData);
            } else {
                // If not in local storage, make an API request to fetch the transaction details
                const endPoint = `https://apilist.tronscanapi.com/api/transaction-info?hash=${this.transactionHash}`;
				const headers = {
					'TRON-PRO-API-KEY': '733cc233-20ad-43c6-85ef-1b109e58873d',
				};

                axios
                    .get(endPoint, { headers })
                    .then((response) => {
						if (response.data.contractRet == 'SUCCESS') {
							this.transactionData = response.data;
							localStorage.setItem(this.transactionHash, JSON.stringify(this.transactionData));
						} else {
							this.transactionData = null;
						}
                    })
                    .catch((error) => {
                        console.error(error);
						this.transactionData = null;
                    });
            }
        },
    },
};
</script>

<style>
.transaction-details {
	padding: 20px;
	border: 1px solid #ddd;
	border-radius: 5px;
}

.detail-row {
	margin-bottom: 10px;
}

.detail-label {
	font-weight: bold;
}

.detail-value {
	margin-left: 10px;
}
</style>
