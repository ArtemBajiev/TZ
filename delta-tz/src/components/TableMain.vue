<template>
  <div class="table-main__container">
    <table class="table-main">
      <tr class="table-main__header">
        <th class="table-main__header__item">Показатель</th>
        <th class="table-main__header__item" v-for="item in tableData.tableHeader" :key="item">
          {{ item }}
        </th>
      </tr>
      <tr class="table-main__total-revenue" @click="dataGraph(tableData.TotalRevenue)">
        <td class="table-main__total-revenue__item">{{ tableData.TotalRevenue.type }}</td>
        <td class="table-main__total-revenue__item">{{ tableData.TotalRevenue.now }}</td>
        <td class="table-main__total-revenue__item" :class="{'table-Less-than-today': tableData.TotalRevenue.yesterday.procent > 0, 'more-than-today': tableData.TotalRevenue.yesterday.procent < 0,}">{{ tableData.TotalRevenue.yesterday.value }}<span class="procent">{{ tableData.TotalRevenue.yesterday.procent+'%' }}</span></td>
        <td class="table-main__total-revenue__item" :class="{'table-Less-than-today': tableData.TotalRevenue.dayWeek.procent > 0, 'more-than-today': tableData.TotalRevenue.dayWeek.procent < 0,}">{{ tableData.TotalRevenue.dayWeek.value }}<span class="procent">{{ tableData.TotalRevenue.dayWeek.procent+'%' }}</span></td>
        <td class="table-main__total-revenue__item" :class="{'table-Less-than-today': tableData.TotalRevenue.tomorrow.procent > 0, 'more-than-today': tableData.TotalRevenue.tomorrow.procent < 0,}">{{ tableData.TotalRevenue.tomorrow.value }}<span class="procent">{{ tableData.TotalRevenue.tomorrow.procent+'%' }}</span></td>
      </tr>
      <tr class="table-main__graph-container">
        <GraphCompVue :key="updateKey" :chartDataP="chartData"></GraphCompVue>
      </tr>
      <tr
        class="table-main__line-elements"
        v-for="item in tableData.tableValues"
        :key="item.id"
        @click="dataGraph(item)"
      >
        <td class="table-main__line-element">{{ item.type }}</td>
        <td class="table-main__line-element">{{ item.now }}</td>
        <td
          class="table-main__line-element"
          :class="{'table-Less-than-today': item.yesterday.procent > 0, 'more-than-today': item.yesterday.procent < 0,}"
        >
          {{ item.yesterday.value
          }}<span class="procent">{{ item.yesterday.procent+'%' }}</span>
        </td>
        <td
          class="table-main__line-element"
          :class="{'table-Less-than-today': item.dayWeek.procent > 0, 'more-than-today': item.dayWeek.procent < 0,}"
        >
          {{ item.dayWeek.value }}
          <span class="procent">{{ item.dayWeek.procent+'%' }}</span>
        </td>
        <td
          class="table-main__line-element"
          :class="{'table-Less-than-today': item.tomorrow.procent > 0, 'more-than-today': item.tomorrow.procent < 0,}"
        >
          {{ item.tomorrow.value }}
          <span class="procent">{{ item.tomorrow.procent+'%' }}</span>
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
import GraphCompVue from './GraphComp.vue'

export default {
  components: { GraphCompVue },
  data() {
    return {
      updateKey: 0,
      tableData: {
        tableHeader: [ 'Текущий день', 'Вчера', 'Этот день недели', 'Ожидания завтра'],
        tableValues: [
          { type: 'Наличные', now: '300 000', yesterday: {value:'300 000',procent: 0}, dayWeek: {value:'350 521',procent: 6},tomorrow: {value:'510 521',procent: -20}, id: 1 },
          {
            type: 'Безналичный расчёт',
            now: '100 000',
            yesterday: {value:'100 222',procent: 2},
            dayWeek: {value:'100 000',procent: 0},
            tomorrow: {value:'80 521',procent: -19},
            id: 2
          },
          {
            type: 'Кредитные карты',
            now: '100 521',
            yesterday: {value:'480 521',procent: 5},
            dayWeek: {value:'480 521',procent: 5},
            tomorrow: {value:'480 521',procent: 5},
            id: 3
          },
          { type: 'Средний чек, руб', now: '1 300', yesterday: {value:'480 521',procent: 5}, dayWeek: {value:'480 521',procent: 5}, tomorrow: {value:'480 521',procent: 5},id: 4},
          { type: 'Средний гость, руб', now: '1 200', yesterday: {value:'480 521',procent: 5}, dayWeek: {value:'480 521',procent: 3},tomorrow: {value:'480 521',procent: 5}, id: 5 },
          {
            type: 'Удаления из чека (после оплаты), руб',
            now: '300 000',
            yesterday: {value:'380 521',procent: 5},
            dayWeek: {value:'299 521',procent: 0},
            tomorrow: {value:'480 521',procent: 0},
            id: 6
          },
          {
            type: 'Удаления из счета (до оплаты), руб',
            now: '1 300',
            yesterday: {value:'521',procent: 56},
            dayWeek: {value:'1 521',procent: -7},
            tomorrow: {value:'1 221',procent: 2},
            id: 7
          },
          { type: 'Количество чеков', now: '34', yesterday: {value:'480 521',procent: 5}, dayWeek: {value:'480 521',procent: 5},tomorrow: {value:'480 521',procent: 5}, id: 8 },
          { type: 'Наличные', now: '34', yesterday: {value:'38',procent: 1}, dayWeek: {value:'30',procent: -3},tomorrow: {value:'40',procent: -5}, id: 9 }
        ],
        TotalRevenue: { type: 'Выручка', now: '500 521', yesterday: {value:'480 521',procent: 5}, dayWeek: {value:'520 000',procent: -5},tomorrow: {value:'430 521',procent: 10}, }
      },
      chartData: {
        labels: [],
        datasets: [
          {
            label: 'Data One',
            backgroundColor: '#f87979',
            data: []
          }
        ]
      }
    }
  },
  methods: {
    dataGraph(item) {
      this.chartData = {
        labels: this.tableData.tableHeader,
        datasets: [
          {
            label: '',
            backgroundColor: '#4bacda',
            data: []
          }
        ]
      };
      (this.chartData.datasets[0].data = [
        Number(item.now.replace(/\s/g, '')),
        Number(item.yesterday.value.replace(/\s/g, '')),
        Number(item.dayWeek.value.replace(/\s/g, '')),
        Number(item.tomorrow.value.replace(/\s/g, ''))
      ]),
        (this.chartData.datasets[0].label = item.type)
      this.updateKey = this.updateKey++
    },
   
  },
  created() {
    this.dataGraph(this.tableData.TotalRevenue)
  },
  computed: {
 
  }
}
</script>
<style lang="scss">
.table-main__container
{
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.table-main {
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.212);
}
.table-main__header__item {
  padding: 20px 5px;
  margin: 2px;
  background-color: #f9f9f9;
}
.table-main__graph-container {
  max-width: 100%;
  position: relative;
  height: 200px;
}
.table-main__total-revenue__item {
  text-align: center;
  padding: 10px 0px;
  max-width: 180px;
  min-width: 100px;
  margin: 2px;
  background-color: #f9f9f9;
}
.table-main__line-element {
  text-align: center;
  padding: 10px 0px;
  max-width: 180px;
  min-width: 100px;
  margin: 2px;
  background-color: #f9f9f9;
}
.table-main__header__item:nth-child(2),
.table-main__total-revenue__item:nth-child(2),
.table-main__line-element:nth-child(2) {
  background-color: #edfaff;
}
.table-Less-than-today {
  background-color: #e7f6df;
}
.more-than-today{
    background-color: #fadbd8;
}
.procent {
  margin-left: 5px;
}
@media (hover: hover) {
  .table-main__total-revenue *{
    transition: all 0.3s ease-in-out;
  }
  .table-main__line-elements > *{
  transition: all 0.3s ease-in-out;
}
  .table-main__total-revenue:hover > * {
    cursor: pointer;
    background-color: #bebebe65;
  }
  .table-main__line-elements:hover > *
  {
    cursor: pointer;
    background-color: #bebebe65;
  }
}
@media (max-width: 600px) {
  .table-main__line-element
  {
    max-width: 180px;
    min-width: 10px;
    font-size: 11px;
  }
  .table-main__total-revenue__item{
    max-width: 180px;
    min-width: 10px;
    font-size: 11px;
  }
}
</style>
