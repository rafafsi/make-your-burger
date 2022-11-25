<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="burger-form" @submit="createBurger($event)">
      <div class="input-container">
        <label for="name">Customer's name: </label>
        <input
          type="text"
          name="name"
          id="name"
          v-model="customer"
          placeholder="Type your name"
        />
      </div>

      <div class="input-container">
        <label for="bread">Bread:</label>
        <select name="bread" id="bread" v-model="bread">
          <option value="">Select your bread</option>
          <option v-for="bread in breads" :key="bread.id" :value="bread.type">
            {{ bread.type }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="meat">Meat:</label>
        <select name="meat" id="meat" v-model="meat">
          <option value="">Select your bread</option>
          <option v-for="meat in meats" :key="meat.id" :value="meat.type">
            {{ meat.type }}
          </option>
        </select>
      </div>

      <div id="optional-container" class="input-container">
        <label id="optional-title" for="optinonal"
          >Select the optionals:
        </label>
        <div
          class="checkbox-container"
          v-for="optional in optionalsData"
          :key="optional.id"
        >
          <input
            type="checkbox"
            name="optional"
            id="optional"
            v-model="optionals"
            :value="optional.type"
          />
          <span>{{ optional.type }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="create my burger" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",
  components: { Message },
  data() {
    return {
      breads: "",
      meats: "",
      optionalsData: "",
      customer: null,
      bread: null,
      meat: null,
      optionals: [],
      status: "",
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredients");
      const data = await req.json();

      this.breads = data.breads;
      this.meats = data.meats;
      this.optionalsData = data.optionals;
    },

    async createBurger(e) {
      e.preventDefault();

      const data = {
        customer: this.customer,
        bread: this.bread,
        meat: this.meat,
        optionals: Array.from(this.optionals),
        status: "required",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      console.log(res);
      this.msg = `Your order nº${res.id} has been done! `;
      this.customer = "";
      this.bread = "";
      this.meat = "";
      this.optionals = [];

      setTimeout(() => {
        this.msg = ""
      }, 3000);
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}
.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#optional-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#optional-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}
.checkbox-container span,
.checkbox-container input {
  width: auto;
}
.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>

