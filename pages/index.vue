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
            autofocus="autofocus"
            @keyup.enter="
              (e) => {
                e.target.blur();
              }
            "
          />
        </div>
      </div>
    </div>

    <div class="col-md-12">
      <label for="">
        From {{ calculate.currency }} to {{ calculate.to }} Price[{{
          calculate.toAmount
        }}]
      </label>
    </div>
    <div class="col-md-12">
      <table class="table table-responsive">
        <tbody>
          <tr>
            <td></td>
            <td>{{ calculate.currency }}</td>
            <td>{{ calculate.to }}</td>
          </tr>
          <tr>
            <td class="w-25">Per Hour</td>
            <td class="">{{ formatCurrency(perHour()) }}</td>
            <td class="">{{ formatCurrency(perHour(true), true) }}</td>
          </tr>
          <tr>
            <td class="w-25">Per Day</td>
            <td class="">{{ formatCurrency(perDay()) }}</td>
            <td class="">{{ formatCurrency(perDay(true), true) }}</td>
          </tr>
          <tr>
            <td class="w-25">Per Week</td>
            <td class="">{{ formatCurrency(perWeek()) }}</td>
            <td class="">{{ formatCurrency(perWeek(true), true) }}</td>
          </tr>
          <tr>
            <td class="w-25">Per Month</td>
            <td class="">{{ formatCurrency(perMonth()) }}</td>
            <td class="">{{ formatCurrency(perMonth(true), true) }}</td>
          </tr>
          <tr>
            <td class="w-25">Per Year</td>
            <td class="">{{ formatCurrency(perYear()) }}</td>
            <td class="">
              {{ formatCurrency(perYear(true), true) }}
            </td>
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
    useHead({
      title: "Calculate",
      //   script: [{ src: "https://code.jquery.com/jquery-3.6.0.min.js" }],
    });
    const currencies = ref(countryCodes);
    const calculate = reactive({
      currency: "USD",
      to: "INR",
      amount: null,
      toAmount: 79.290504,
    });
    onMounted(async () => {
      // getConvertToAmount();
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

    async function getConvertToAmount() {
      if (calculate.currency == "" && calculate.to == "") {
        return false;
      }
      const url = await $fetch(
        `https://api.apilayer.com/currency_data/convert?to=${calculate.to}&from=${calculate.currency}&amount=1`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            //   "Access-Control-Allow-Origin": "*",
            apikey: "",
          },
          async onResponse({ request, response, options }) {
            if (response._data?.success) {
              calculate.toAmount = response._data?.result;
            }
          },
        }
      );
    }

    function formatCurrency(value, to = false) {
      if (value == "" || value == null) {
        return value;
      }
      if (!to) {
        return value.toLocaleString("en-US", {
          maximumFractionDigits: 2,
          style: "currency",
          currency: "USD",
        });
      }
      return value.toLocaleString("en-IN", {
        maximumFractionDigits: 2,
        style: "currency",
        currency: "INR",
      });
    }

    function perHour(to = false) {
      if (!to) {
        return calculate.amount;
      }
      return calculate.amount * calculate.toAmount;
    }

    function perDay(to = false) {
      if (!to) {
        return calculate.amount * 8;
      }
      return calculate.amount * 8 * calculate.toAmount;
    }

    function perWeek(to = false) {
      if (!to) {
        return perDay() * 5;
      }
      return perDay(true) * 5;
    }

    function perMonth(to = false) {
      if (!to) {
        return perWeek() * 4;
      }
      return perWeek(true) * 4;
    }

    function perYear(to = false) {
      if (!to) {
        return perMonth() * 12;
      }
      return perMonth(true) * 12;
    }

    return {
      currencies,
      calculate,
      formatCurrency,
      perDay,
      perWeek,
      perHour,
      perMonth,
      perYear,
    };
  },
};
</script>

