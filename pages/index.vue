<template>
  <div class="p-3">
    <div class="row">
      <div class="col-md-12 text-center">Calculate</div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <div class="input-group mb-3">
          <select
            name="currency"
            id="currency"
            class="input-group-text"
            v-model="calculate.currency"
          >
            <option
              v-for="(cval, ckey) in currencies"
              :key="ckey"
              :value="ckey"
              :title="cval"
            >
              {{ ckey }}
            </option>
          </select>
          <input
            type="number"
            class="form-control"
            id="basic-url"
            v-model="calculate.amount"
          />
        </div>
      </div>
    </div>

    <div class="col-md-12">
      <label for="">
        From {{ calculate.currency }} to {{ calculate.to }}
      </label>
    </div>
    <div class="col-md-12">
      <table class="table table-responsive">
        <tbody>
          <tr>
            <td class="w-25">Per Day</td>
            <td class="">{{ calculate.amount * 8 }}</td>
            <td class="">{{ calculate.amount * 8 * 78 }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import { ref, onMounted } from "vue";
import countryCodes from "../public/currencies";
export default {
  setup() {
    const currencies = ref(countryCodes);
    const calculate = reactive({
      currency: "USD",
      to: "INR",
      amount: null,
    });
    onMounted(async () => {
      //   const url = await $fetch("https://api.apilayer.com/currency_data/list", {
      //     method: "GET",
      //     headers: {
      //       "Content-Type": "application/json",
      //       //   "Access-Control-Allow-Origin": "*",
      //       apikey: "",
      //     },
      //     async onResponse({ request, response, options }) {
      //       currency.value = response._data?.currencies;
      //     },
      //   });
    });
    return {
      currencies,
      calculate,
    };
  },
};
</script>

