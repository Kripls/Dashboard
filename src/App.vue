<template>
  <div id="app">
    <div class="dashboardTop clearfix">
      <ul class="filterDashboardList">
        <li>
          <a v-bind:class="{success: activeLink === 0}" class="v-btn theme--light"  @click.prevent="valueFilterDataClick($event, 0)">Все задачи</a>
        </li>
        <li>
          <a v-bind:class="{success: activeLink === 1}" class="v-btn theme--light" @click.prevent="valueFilterDataClick($event, 1)">Провести проверку</a>
        </li>
        <li>
          <a v-bind:class="{success: activeLink === 2}" class="v-btn theme--light" @click.prevent="valueFilterDataClick($event, 2)">Контроль предписания</a>
        </li>
        <li>
          <a v-bind:class="{success: activeLink === 3}" class="v-btn theme--light" @click.prevent="valueFilterDataClick($event, 3)">Поручения</a>
        </li>
      </ul>
      <ul class="dashboardElemCount">
        <li class="v-btn--floating--custom blue">{{ status_id_count[1] + status_id_count[2] + status_id_count[3]}}</li>
        <li class="v-btn--floating--custom green">{{ status_id_count[1] }}</li>
        <li class="v-btn--floating--custom yellow">{{ status_id_count[2] }}</li>
        <li class="v-btn--floating--custom red">{{ status_id_count[3] }}</li>
      </ul>
    </div>

    <h4 class="headline v-card__text blue">Задачи на исполнение</h4>
    <table class="dashboardList v-datatable v-table theme--light">
      <thead>
        <tr>
          <th class="subheading column text-xs-left">
            <span>Срок</span>
          </th>
          <th class="subheading column text-xs-left">
            <span>Исполнитель</span>
          </th >
          <th class="subheading column text-xs-left">
            <span>Тип</span>
          </th>
          <th class="subheading column text-xs-left">
            <span>Краткое содержание</span>
          </th>
          <th class="subheading column text-xs-left">
            Дата создания задачи
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="el in filteredDashboard">
          <td>
            <span v-bind:class="[status_id_color[el.status_id]]" class="text-date">{{ el.require_date }}</span>
          </td>
          <td>
            <span>{{ el.executor }}</span>
          </td>
          <td>
            <span>{{ type_id_name[el.type_id] }}</span>
          </td>
          <td>
            <span>{{ el.description }}</span>
          </td>
          <td>
            <span>{{ el.date_create }}</span>
          </td>
        </tr>
      </tbody>

    </table>
  </div>
</template>

<script>
  import _ from 'lodash'
  const userData = require(`./assets/data.json`);

  export default {
    name: 'app',
    data () {
      return {
        filterData: [],
        valueFilterData: 0,
        status_id_color: {
          1: "green",
          2: "yellow",
          3: "red"
        },
        status_id_count: {
          1: 0,
          2: 0,
          3: 0
        },
        activeLink: 0,
        type_id_name: {
          1: 'Провести проверку',
          2: 'Контроль предписания',
          3: 'Поручение'
        },
        jsonDate: userData
      }
    },
    computed: {

      filteredDashboard () {

        this.filterData = [];
        this.status_id_count[1] = 0;
        this.status_id_count[2] = 0;
        this.status_id_count[3] = 0;
          for (let el of this.jsonDate) {

            if (el.type_id == this.valueFilterData) {

              this.status_id_count[el.status_id] += 1;

              this.filterData.push(el);

            } else if (this.valueFilterData == 0) {
              this.status_id_count[el.status_id] += 1;

              this.filterData.push(el);
            }

          }

        return this.filterData;

      }

    },
    methods: {

      valueFilterDataClick(el, value) {
        this.activeLink = value;
        this.valueFilterData = value;
      }

    }
  }
</script>

<style>
  ol, ul {
    margin: 0;
    padding: 0;
  }
  li {
    list-style-type: none;
    display: inline-block;
  }
  a {
    cursor: pointer;
  }
  .clearfix:before, .clearfix:after {
    clear: both;
    display: table;
    content: "";
  }
  .success {
    background-color: #4caf50!important;
    border-color: #4caf50!important;
  }
  .headline {
    color: #fff;
  }
  .dashboardTop ul{
    width: 50%;
    float: left;
  }
  .dashboardElemCount {
    text-align: center;
  }
  .v-btn--floating--custom {
    border-radius: 50%;
    min-width: 0;
    height: 25px;
    width: 25px;
    padding: 0;
    line-height: 25px;
    text-align: center;
    color: #fff;
  }
  .text-date {
    padding: 8px 15px;
    border-radius: 4px;
    color: #fff;
  }
</style>
