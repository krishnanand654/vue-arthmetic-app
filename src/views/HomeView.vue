<template>
  <AppNavbar />
  <div class="calc-main-ctn">
    <img src="../assets/calc.png" class="calc-img" />
    <div class="calc-field-ctn">
      <div>
        <input type="number" v-model="num1" required placeholder="Enter number 1" />
      </div>
      <div class="calc-op">
        <h1 v-if="operation === 'add'">+</h1>
        <h1 v-if="operation === 'subtract'">-</h1>
        <h1 v-if="operation === 'multiply'">x</h1>
        <h1 v-if="operation === 'divide'">/</h1>
      </div>
      <div>
        <input type="number" v-model="num2" required placeholder="Enter number 2" />
      </div>
    </div>
    <h1>{{ result }}</h1>
    <div class="buttons">
      <button @click="fetchOperation('add')">+</button>
      <button @click="fetchOperation('subtract')">-</button>
      <button @click="fetchOperation('multiply')">x</button>
      <button @click="fetchOperation('divide')">/</button>
    </div>
    <p>{{ error }}</p>
    <h2 class="his-title" v-if="op_array.length > 0">History</h2>
    <div>
      <ul>
        <li v-for="history in op_array.slice().reverse()" :key="history">
          {{ history }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import AppNavbar from '@/components/AppNavbar.vue'

export default {
  name: 'HomeView',
  components: {
    AppNavbar
  },
  data() {
    return {
      num1: null,
      num2: null,
      error: null,
      operation: '',
      result: null,
      op_array: []
    }
  },
  methods: {
    fetchOperation(operation) {
      this.operation = operation

      const token = localStorage.getItem('token')
      axios
        .post(
          `http://localhost:3000/${operation}`,
          { num1: this.num1, num2: this.num2 },
          {
            headers: {
              Authorization: `Bearer ${token}`
            }
          }
        )
        .then((response) => {
          this.result = '= ' + response.data.result
          let history =
            this.num1 +
            (operation == 'add'
              ? ' + '
              : operation == 'subtract'
                ? ' - '
                : operation == 'multiply'
                  ? ' * '
                  : ' / ') +
            this.num2 +
            this.result
          this.op_array.push(history)
          this.error = null
          console.log(this.op_array)
        })
        .catch((error) => {
          this.error = error.response.data.error
        })
    }
  }
}
</script>

<style>
.calc-main-ctn {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 30px;
  padding-top: 160px;
  overflow-y: auto;
}

.calc-img {
  width: auto;
  height: 50px;
}

.calc-field-ctn {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}

.calc-op {
  position: relative;
  top: -5px;
}

.his-title {
  font-weight: 500;
  border-bottom: 2px solid #7e7e7ea0;
}

.calc-field-ctn input {
  background: #2f2f2f;
  border: none;
  height: 40px;
  border-radius: 50px;
  color: white;
  margin: 5px 0 10px;
  font-weight: 500;
  font-size: 18px;
  padding: 0 20px 0;
  outline: none;
  width: 200px;
  transition: 0.5s ease-in-out;
}

input:focus {
  outline: 2px solid #29a569;
  transform: translateY(-2px);
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.buttons button {
  background: #2f2f2f;
  color: white;
  border: none;
  border-radius: 50%; /* Use 50% for circle */
  width: 50px;
  height: 50px;
  font-size: 24px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #999999;
}

.buttons button:hover {
  background: #29a569;
  color: white;
}

.buttons button:focus {
  background: #29a569;
  color: white;
}

input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type='number'] {
  -moz-appearance: textfield; /* Firefox */
}

ul {
  list-style: none;
  text-align: center;
  font-size: 18px;
  line-height: 50px;
  padding: 0;
  margin-bottom: 50px;
}

@media only screen and (max-width: 768px) {
  .calc-field-ctn {
    flex-direction: column;
    align-items: center;
  }
}
</style>
