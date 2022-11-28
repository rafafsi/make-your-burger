<template>
    <Message :msg="msg" v-show="msg"/>
    <div v-if="burgers" id="burger-table" >
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Customer:</div>
                <div>Bread:</div>
                <div>Meat:</div>
                <div>Optionals:</div>
                <div>Actions:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{burger.id}}</div>
                <div>{{burger.customer}}</div>
                <div>{{burger.bread}}</div>
                <div>{{burger.meat}}</div>
                <div>
                    <ul v-for="(optional, idx) in burger.optionals" :key="idx">
                        <li>{{optional}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateOrder($event, burger.id)">
                        <option v-for="s in status" 
                                :value="s.type" 
                                :key="s.id"
                                :selected="burger.status == s.type"> 
                                {{s.type}} </option>
                    </select>
                    <Button  @click="deleteOrder(burger.id)" :txt="btn"/>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <h2>there is no orders</h2>
    </div>
</template>

<script>
import Message from './Message.vue'
import Button from './Button.vue'

export default {
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null,
            btn: "Cancel"
        }
    },
    components: { Message, Button },
    methods: {
        async getOrders() {
            const req = await fetch('http://localhost:3000/burgers')
            const data = await req.json()

            this.burgers = data;
        },
        async getStatus() {
            const req = await fetch('http://localhost:3000/status')
            const data = await req.json()
            
            this.status = data
        },
        async updateOrder(e, id) {

            const option = e.target.value;
            const dataJson = JSON.stringify({status: option})

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'PATCH',
                headers: {"Content-Type": "application/json"},
                body: dataJson
            })

            const res = await req.json()
        },
        async deleteOrder(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'DELETE'
            })

            const res = await req.json()
            this.getOrders()
            
            this.msg = `Your order Nº${id} has been successfully canceled!`
            
            setTimeout(() => {
                this.msg = ""
            }, 3000)


        }
    },
    mounted() {
        this.getOrders()
        this.getStatus()
    }
    
}
</script>

<style scoped>
      #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }
  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }
  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }
  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }
  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

</style>