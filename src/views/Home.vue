<template>
  <Layout>
    <template #header>
        <Header></Header>
    </template>
    <template #resume>
        <Resume
          :label="'Ahorro total'"
          :total-amount="totalAmount"
          :amount="amount">
          <template #graphic>
            <Graphic :amounts="amounts" />
          </template>
          <template #action>
            <Actions @create="create"/>
          </template>
        </Resume>
    </template>
    <template #movements>
        <Movements
          @remove="remove"
          :movements="movements" />
    </template>
  </Layout>
</template>

<script>
import Layout from '../components/Layout.vue';
import Header from '../components/Header.vue';
import Resume from '../components/Resume.vue';
import Actions from '../components/Actions.vue';
import Movements from '../components/Movements.vue';
import Graphic from '../components/Graphic.vue';


export default {
  components: {
      Layout,
      Header,
      Resume,
      Movements,
      Actions,
      Graphic,
  },
  data() {
      return {
          amount: null,
          movements: [{
              id: 1,
              title: "Movimiento",
              description: "Deposito de salario",
              amount: 100,
              time: new Date('9-28-2022'),
            },
            {
              id: 2,
              title: "Movimiento 1",
              description: "Deposito de honorarios",
              amount: 300,
              time: new Date('9-29-2022'),
            },
            {
              id: 3,
              title: "Movimiento 3",
              description: "Comida",
              amount: 800,
              time: new Date('9-30-2022'),
            },
            {
              id: 4,
              title: "Movimiento 4",
              description: "Colegiatura",
              amount: 1000,
              time: new Date('10-2-2022'),
            },
            {
              id: 5,
              title: "Movimiento 5",
              description: "Reparación equipo",
              amount: 600,
              time: new Date('10-3-2022'),
            },
            {
              id: 6,
              title: "Movimiento 6",
              description: "Reparación equipo",
              amount: 100,
              time: new Date('10-4-2022'),
            },
            {
              id: 7,
              title: "Movimiento 7",
              description: "Reparación equipo",
              amount: 1000,
              time: new Date('01-01-2022'),
            },
            {
              id: 8,
              title: "Movimiento 8",
              description: "Reparación equipo",
              amount: 1000,
              time: new Date('01-01-2022'),
            },
          ],
      }
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));
    if(Array.isArray(movements)) {
      this.movements = movements?.map(item => (
        { ...item, time: new Date(item.time) }
      ));
    }
  },
  methods: {
    create(movement) {
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex(m => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
  },
  computed: {
    totalAmount() {
      return this.movements.reduce((suma, movement) => {
        return suma + movement.amount;
      }, 0);
    },
    amounts() {
      const lastDays = this.movements
        .filter((move) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);
          
          return move.time > oldDate;
        })
        .map((move) => move.amount);

      return lastDays.map((move, i) => {
        const lastMovements = lastDays.slice(0, i);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
  }
};
</script>
