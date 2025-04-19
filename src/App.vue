<template>
  <div :class="['container', isDarkMode ? 'dark-mode' : 'light-mode']">
    <header class="header">
      <h2 style="font-size: 40px;">MyFinanceHub</h2>
      <button @click="toggleTheme" class="theme-toggle">{{ isDarkMode ? '🌙' : '☀️' }}</button>
    </header>
    
    <div class="balance">
      <h3 style="font-size: 35px;">Мой баланс:</h3>
      <p style="font-size: 30px; font-weight: 10  ;">{{ totalProfit - totalExpanse }}$</p>
    </div>
    
    <div class="input-sections">
      <div class="profit_div">
        <h3 style="font-size: 30px;margin: 10px 0 ;">Доход</h3>
        <input class="input_profit" v-model="newProfitName" type="text" placeholder="Доход:">
        <input class="number_profit" v-model.number="newProfitValue" type="number" placeholder="0">
        <button @click="addNewProfit" class="button1 btn">Добавить доход</button>
      </div>
      
      <div class="expanse_div">
        <h3 style="font-size: 30px;margin: 10px 0 ;">Расход</h3>
        <input class="input_expanse" v-model="newExpanseName" type="text" placeholder="Расход:">
        <input class="number_expanse" v-model.number="newExpanseValue" type="number" placeholder="0">
        <select  v-model="newExpanseCategory" class="service_select">
          <option value="Food">Еда</option>
          <option value="Clothing">Одежда</option>
          <option value="Transport">Транспорт</option>
          <option value="Other">Остальное</option>
        </select>
        <button @click="addNewExpanse" class="button2 btn">Добавить расход</button>
      </div>
    </div>

    <div class="stats">
      <h3>Статистика</h3>
      <button @click="sortedProfits" class="btn">Общий доход = {{ totalProfit }}</button>
      <button @click="sortedExpanses" class="btn">Общий расход = {{ totalExpanse }}$</button>
      <button @click="showLargestExpense" class="btn">Самый большой расход = {{ largestExpense }}$</button>
    </div>

    <div class="lists">
      <div class="profit_articles">
        <h3>Доходы</h3>
        <div v-for="(profit, index) in sortedProfits" :key="index" class="card">
          <p>{{ profit.name }} - {{ profit.value }}$ - {{ profit.date }}</p>
          <button @click="deleteProfit(index)" class="delete-btn">Удалить</button>
        </div>
      </div>

      <div class="expanse_articles">
        <h3>Расходы</h3>
        <div v-for="(expanse, index) in sortedExpanses" :key="index" class="card">
          <p>{{ expanse.name }} - {{ expanse.value }}$ - {{ expanse.date }} - {{ expanse.category }}</p>
          <button @click="deleteExpanse(index)" class="delete-btn">Удалить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isDarkMode = ref(true);
const toggleTheme = () => { isDarkMode.value = !isDarkMode.value; };

const expanses = ref([]);
const profits = ref([]);
const totalExpanse = ref(0);
const totalProfit = ref(0);

const newProfitName = ref('');
const newProfitValue = ref(0);
const newExpanseName = ref('');
const newExpanseValue = ref(0);
const newExpanseCategory = ref('Food');

const addNewProfit = () => {
  if (newProfitValue.value > 0) {
    profits.value.push({ name: newProfitName.value, value: newProfitValue.value, date: new Date().toLocaleString() });
    totalProfit.value += newProfitValue.value;
    newProfitName.value = '';
    newProfitValue.value = 0;
  }
};

const addNewExpanse = () => {
  if (newExpanseValue.value > 0) {
    expanses.value.push({ name: newExpanseName.value, value: newExpanseValue.value, date: new Date().toLocaleString(), category: newExpanseCategory.value });
    totalExpanse.value += newExpanseValue.value;
    newExpanseName.value = '';
    newExpanseValue.value = 0;
  }
};

const deleteProfit = (index) => {
  totalProfit.value -= profits.value[index].value;
  profits.value.splice(index, 1);
};

const deleteExpanse = (index) => {
  totalExpanse.value -= expanses.value[index].value;
  expanses.value.splice(index, 1);
};

const sortedProfits = computed(() => [...profits.value].sort((a, b) => b.value - a.value));
const sortedExpanses = computed(() => [...expanses.value].sort((a, b) => b.value - a.value));
const largestExpense = computed(() => expanses.value.length ? Math.max(...expanses.value.map(exp => exp.value)) : 0);
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  border-radius: 12px;
  transition: all 0.3s;
}
.balance{
  border-radius: 18px;
  border: 2px solid ;
  padding: 0 30px;
  text-align: center;
  margin-bottom: 30px;
}
.dark-mode {
  background: black;
  color: white;
}

.light-mode {
  background: white;
  color: black;
}
.btn, .delete-btn {
  background-color: var(--btn-bg);
  color: var(--btn-color);
  /* border: 2px solid; */
  transition: background-color 0.3s, color 0.3s;
}
.button1  {
  width: 92.5%;
  border: 2px solid  ;
  padding: 10px 20px;
  margin-top: 10px;
}
.button2{
  width: 92.5%;
  border: 2px solid  ;
  padding: 10px 20px;
  margin-top: 10px;
}
.input_profit{
  width: 90%;
  font-size: 15px;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
}
.input_expanse{
  width: 90%;
  font-size: 15px;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
}
.number_profit{
  width: 90%;
  font-size: 15px;
  padding: 10px;
  border-radius: 8px;
}
.number_expanse{
  width: 90%;
  font-size: 15px;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
}

.service_select{
  width: 92.5%;
  font-size: 15px;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
}
.dark-mode {
  --btn-bg: black;
  --btn-color: white;
}

.light-mode {
  --btn-bg: white;
  --btn-color: black;
}

.delete-btn {
  background-color: red;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.theme-toggle {
  cursor: pointer;
  background: none;
  border: none;
  font-size: 24px;
}

.input-sections {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.profit_div, .expanse_div {
  padding: 15px;
  border-radius: 12px;
  border: 2px solid;
}

.card {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border: 1px solid;
  border-radius: 8px;
  margin: 5px 0;
}


@media (max-width: 600px) {
  .input-sections {
    flex-direction: column;
  }
  .service_select{

  }
  .btn, .delete-btn {
    width: 92.5%;
  }
}
</style>
