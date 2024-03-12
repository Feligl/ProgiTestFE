<template>
    <form @submit.prevent="handleSubmit">
        <label>Vehicle Price</label>
        <input type="number" min=1 v-model="vehiclePrice" required />

        <label>Vehicle Type</label>
        <select v-model="vehicleType">
            <option value="1">Common</option>
            <option value="2">Luxury</option>
        </select>

        <div class="submit">
            <button>Calculate Final Price</button>
        </div>

        <div v-if="apiData">
            <label>Fees</label>
            <table>
                <tr>
                    <th>Basic Fee: </th>
                    <td>{{ apiData.basicUserFee }}</td>
                </tr>
                <tr>
                    <th>Special Fee: </th>
                    <td>{{ apiData.specialFee }}</td>
                </tr>
                <tr>
                    <th>Association Fee: </th>
                    <td>{{ apiData.associationFee }}</td>
                </tr>
                <tr>
                    <th>Storage Fee: </th>
                    <td>{{ apiData.storageFee }}</td>
                </tr>
                <tr>
                    <th>Total Price: </th>
                    <td>{{ apiData.totalPrice }}</td>
                </tr>
            </table>
        </div>
    </form>
</template>
<script>
export default {
    data() {
        return {
            vehiclePrice: 1,
            vehicleType: 1,
            apiData: null
        }
    },
    methods: {
        async handleSubmit() {
            this.apiData = null;

            try {
                const apiRequest = await fetch(`http://localhost:2000/VehicleAuction/CalculateTotalAuctionPrice?vehicleBasePrice=${this.vehiclePrice}&vehicleType=${this.vehicleType}`);
                this.apiData = await apiRequest.json();
            } catch (error) {
                console.log(error);
            }
        }
    }
}
</script>
<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input,
select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}

table {
    padding: 8px;
    width: 100%;
    border: 1px solid #ddd;
    border-radius: 10px;
}

th,
td {
    width: 50%;
    border-bottom: 1px solid #ddd;
    border-collapse: collapse;
}

tr:nth-last-child(1) th,
tr:nth-last-child(1) td {
    border: 0;
}

button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}

.submit {
    text-align: center;
}
</style>