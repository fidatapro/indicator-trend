<template>
  <div>
    <v-data-table
      :headers="headers_"
      :items="data_table"
      sort-by="calories"
      class="custom_table_class"
      :options.sync="pagination"
      @update:page="updatePage()"
      :footer-props="footerProps"
      :loading="loading"
    >
      <!-- hide-default-header -->
      <template v-slot:top>
        <v-toolbar flat class="py-2">
          <v-toolbar-title>Trend table</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <div style="width: 400px" class="d-flex align-center flex-grow-0">
            <v-text-field
              label="Search ..."
              append-icon="mdi-magnify"
              hide-details
              style="width: 50px !important"
              v-model="form.search"
            ></v-text-field>
          </div>
          <v-spacer></v-spacer>
          <v-btn depressed @click="clickFilterColumn()">
            <div class="d-flex">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                role="img"
                aria-hidden="true"
                class="v-icon__svg"
              >
                <path
                  d="M12,15.5A3.5,3.5 0 0,1 8.5,12A3.5,3.5 0 0,1 12,8.5A3.5,3.5 0 0,1 15.5,12A3.5,3.5 0 0,1 12,15.5M19.43,12.97C19.47,12.65 19.5,12.33 19.5,12C19.5,11.67 19.47,11.34 19.43,11L21.54,9.37C21.73,9.22 21.78,8.95 21.66,8.73L19.66,5.27C19.54,5.05 19.27,4.96 19.05,5.05L16.56,6.05C16.04,5.66 15.5,5.32 14.87,5.07L14.5,2.42C14.46,2.18 14.25,2 14,2H10C9.75,2 9.54,2.18 9.5,2.42L9.13,5.07C8.5,5.32 7.96,5.66 7.44,6.05L4.95,5.05C4.73,4.96 4.46,5.05 4.34,5.27L2.34,8.73C2.21,8.95 2.27,9.22 2.46,9.37L4.57,11C4.53,11.34 4.5,11.67 4.5,12C4.5,12.33 4.53,12.65 4.57,12.97L2.46,14.63C2.27,14.78 2.21,15.05 2.34,15.27L4.34,18.73C4.46,18.95 4.73,19.03 4.95,18.95L7.44,17.94C7.96,18.34 8.5,18.68 9.13,18.93L9.5,21.58C9.54,21.82 9.75,22 10,22H14C14.25,22 14.46,21.82 14.5,21.58L14.87,18.93C15.5,18.67 16.04,18.34 16.56,17.94L19.05,18.95C19.27,19.03 19.54,18.95 19.66,18.73L21.66,15.27C21.78,15.05 21.73,14.78 21.54,14.63L19.43,12.97Z"
                ></path>
              </svg>
              <div class="mt-auto">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  role="img"
                  aria-hidden="true"
                  class="v-icon__svg"
                  style="font-size: 14px; height: 14px; width: 14px"
                >
                  <path
                    d="M12,17C10.89,17 10,16.1 10,15C10,13.89 10.89,13 12,13A2,2 0 0,1 14,15A2,2 0 0,1 12,17M18,20V10H6V20H18M18,8A2,2 0 0,1 20,10V20A2,2 0 0,1 18,22H6C4.89,22 4,21.1 4,20V10C4,8.89 4.89,8 6,8H7V6A5,5 0 0,1 12,1A5,5 0 0,1 17,6V8H18M12,3A3,3 0 0,0 9,6V8H15V6A3,3 0 0,0 12,3Z"
                  ></path>
                </svg>
              </div>
            </div>
          </v-btn>
          <v-btn
            depressed
            @click="clickFavorite()"
            :color="check_favorite ? 'primary' : 'default'"
          >
            <div class="d-flex">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                role="img"
                aria-hidden="true"
                class="v-icon__svg"
              >
                <path
                  d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
                ></path>
              </svg>
              <div class="mt-auto">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  role="img"
                  aria-hidden="true"
                  class="v-icon__svg"
                  style="font-size: 14px; height: 14px; width: 14px"
                >
                  <path
                    d="M12,17C10.89,17 10,16.1 10,15C10,13.89 10.89,13 12,13A2,2 0 0,1 14,15A2,2 0 0,1 12,17M18,20V10H6V20H18M18,8A2,2 0 0,1 20,10V20A2,2 0 0,1 18,22H6C4.89,22 4,21.1 4,20V10C4,8.89 4.89,8 6,8H7V6A5,5 0 0,1 12,1A5,5 0 0,1 17,6V8H18M12,3A3,3 0 0,0 9,6V8H15V6A3,3 0 0,0 12,3Z"
                  ></path>
                </svg>
              </div>
            </div>
          </v-btn>
        </v-toolbar>
      </template>
      <!-- eslint-disable-next-line -->
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
      <template v-slot:[`item.icon`]="{ item }">
        <div
          class="d-flex align-center"
          style="border-right: thin solid hsla(0, 0%, 100%, 0.12)"
        >
          <v-btn depressed @click="addFavorite(item)">
            <div
              class="d-flex"
              style="position: -webkit-sticky; position: sticky; left: 0"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                role="img"
                aria-hidden="true"
                class="v-icon__svg"
                :class="{ 'color-favorite': checkFavorite(item) }"
              >
                <path
                  d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
                ></path>
              </svg>
            </div>
          </v-btn>
          <div class="mx-5">
            {{ item.rank }}
          </div>
          <div class="d-flex mx-5">
            <img
              style="width: 30px; padding-right: 8px; vertical-align: middle"
              :src="getSrc(item.coin_symbol)"
            />
            <div class="ml-1 text-start">
              {{ item.coin_name }}
            </div>
          </div>
        </div>
      </template>
      <!-- eslint-disable-next-line -->
      <template v-slot:item.coin_price="{ item }">
        <div
          :class="getStatus(item.status, item.id)"
          class="py-2 d-flex flex-wrap flex-grow-1"
        >
          <span>
            {{ `${getRealValue(item.coin_price)}` }}
          </span>
        </div>
      </template>
      <!-- eslint-disable-next-line -->
      <template v-slot:[`item.technical_score`]="{ item }">
        <span :class="getScoreColor(item.technical_score)">
          {{ `${item.technical_score}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_daily`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_daily)">
          {{ `${item.trend_daily}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_short`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_short)">
          {{ `${item.trend_short}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_mid`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_mid)">
          {{ `${item.trend_mid}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_long`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_long)">
          {{ `${item.trend_long}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_distant`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_distant)">
          {{ `${item.trend_distant}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_4h`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_4h)">
          {{ `${item.trend_4h}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_24h`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_24h)">
          {{ `${item.trend_24h}%` }}
        </span>
      </template>
      <template v-slot:[`item.trend_mean`]="{ item }">
        <span :class="getPersonChangeColor(item.trend_mean)">
          {{ `${item.trend_mean}%` }}
        </span>
      </template>
      <template v-slot:[`item.safety`]="{ item }">
        <span class="normal">
          {{ item.safety }}
        </span>
      </template>
      <template v-slot:[`item.baseline_1h`]="{ item }">
        <span :class="getBaseLineColor(item.coin_price, item.baseline_1h)">
          {{ getRealValue(item.baseline_1h) }}
        </span>
      </template>
      <template v-slot:[`item.support`]="{ item }">
        <span :class="getSportColor(item.coin_price, item.support)">
          {{ `$${getRealValue4(item.support)}` }}
        </span>
      </template>
      <template v-slot:[`item.resistance`]="{ item }">
        <span :class="getResistanceColor(item.coin_price, item.resistance)">
          {{ `$${getRealValue4(item.resistance)}` }}
        </span>
      </template>
    </v-data-table>
    <dialogColumn
      ref="dialogColumn"
      typeColumn="trend"
      :listFields="listFields"
      @reset="initialize()"
    ></dialogColumn>
  </div>
</template>
<script>
import {
  getData,
  getStatus,
  getSrc,
  getPersonChangeColor,
  getScoreColor,
  getSportColor,
  getResistanceColor,
  getRealValue4,
  getRealValue,
  debounce,
  getBaseLineColor,
  flooNumber,
} from "@/views/function";
import JsonSearch from "search-array";
export default {
  components: {
    dialogColumn: () => import("@/views/components/dialog-column.vue"),
  },
  data: () => ({
    getStatus,
    getSrc,
    getRealValue,
    getScoreColor,
    getPersonChangeColor,
    getSportColor,
    getResistanceColor,
    flooNumber,
    getRealValue4,
    getBaseLineColor,
    headers: [
      {
        align: "center",
        sortable: false,
        value: "icon",
        fixed: true,
        text: "NAME",
        width: "350",
      },

      {
        text: "PRICE",
        value: "coin_price",
        align: "start",
        isFilter: true,
        width: "120",
      },
      {
        text: "SCORE",
        value: "technical_score",
        align: "start",
        isFilter: true,
        width: "90",
      }, // duoi 30 do , tren 70 xanh
      {
        text: "TREND DAILY",
        value: "trend_daily",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND SHORT",
        value: "trend_short",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND MID",
        value: "trend_mid",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND LONG",
        value: "trend_long",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND DISTANT",
        value: "trend_distant",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND 4H",
        value: "trend_4h",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND 24H",
        value: "trend_24h",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND MEAN",
        value: "trend_mean",
        align: "end",
        isFilter: true,
        width: "100",
      },

      {
        text: "SAFETY",
        value: "safety",
        align: "end",
        isFilter: true,
        width: "100",
      },
      {
        text: "TREND BASELINE",
        value: "baseline_1h",
        align: "end",
        isFilter: true,
      },
      { text: "SUPPORT", value: "support", align: "end", isFilter: true },
      { text: "RESISTANCE", value: "resistance", align: "end", isFilter: true },
    ],
    pagination: {
      page: 1,
      perpage: 25,
    },
    data_table: [],
    data: [],
    loading: true,
    footerProps: {
      showFirstLastPage: true,
      showCurrentPage: true,
      itemsPerPageOptions: [25, 35, 50, -1],
    },
    form: {
      search: null,
    },
    check_favorite: false,
    headers_: [],
  }),
  watch: {
    "form.search": {
      handler: debounce(function (value) {
        this.handleSearch(value);
      }, 500),
    },
  },
  created() {
    this.initialize();
  },
  methods: {
    async initialize() {
      this.loading = true;
      const coin_data = await getData();
      this.data = coin_data.map((x) => ({
        rank: x.rank,
        coin_symbol: x.coin_symbol,
        coin_name: x.coin_name,
        coin_price: x.coin_price,
        technical_score: x.technical_score,
        trend_daily: x.trend_daily,
        trend_short: x.trend_short,
        trend_mid: x.trend_mid,
        trend_long: x.trend_long,
        trend_distant: x.trend_distant,
        trend_4h: x.trend_4h,
        trend_24h: x.trend_24h,
        trend_mean: x.trend_mean,
        safety: x.safety,
        baseline_1h: x.baseline_1h,
        support: x.support,
        resistance: x.resistance,
      }));
      this.data = this.data.filter((x) => x.technical_score && x.trend_daily);
      this.data_table = this.data;
      this.$refs.dialogColumn.setLisColumnShow();
      this.getColumnShow();
      this.loading = false;
      setInterval(() => this.initialize(), 5 * 60 * 1000);
    },
    getColumnShow() {
      const ema_colum = localStorage.getItem("TREND_COLUMN");
      if (!ema_colum) {
        this.headers_ = this.headers;
      } else {
        const val_heads = JSON.parse(ema_colum).map((x) => x.value);
        this.headers_ = this.headers
          .filter((x) => !x.isFilter)
          .concat(this.headers.filter((x) => val_heads.includes(x.value)));
      }
    },
    async updatePage() {},
    addFavorite({ coin_name }) {
      const list_coin = localStorage.getItem("COIN_FAVORITE");
      if (!list_coin) {
        localStorage.setItem("COIN_FAVORITE", JSON.stringify([coin_name]));
      } else {
        let arr = JSON.parse(list_coin);
        if (arr.includes(coin_name)) {
          arr = arr.filter((x) => x != coin_name);
        } else {
          arr.unshift(coin_name);
        }
        localStorage.setItem("COIN_FAVORITE", JSON.stringify(arr));
      }
      this.initialize();
    },
    handleSearch(text) {
      const searcher = new JsonSearch(this.data);
      this.data_table = searcher.query(text.trim());
    },
    checkFavorite({ coin_name }) {
      const list_coin = localStorage.getItem("COIN_FAVORITE");
      if (!list_coin) return false;
      return JSON.parse(list_coin).includes(coin_name);
    },
    clickFavorite() {
      this.check_favorite = !this.check_favorite;
      if (this.check_favorite) {
        const check_list_coin = localStorage.getItem("COIN_FAVORITE");
        if (!check_list_coin) return;
        const list_coin = JSON.parse(check_list_coin);
        this.data_table = this.data.filter((x) =>
          list_coin.includes(x.coin_name)
        );
      } else {
        this.data_table = this.data;
      }
    },
    clickFilterColumn() {
      this.$refs.dialogColumn.open();
    },
  },
  computed: {
    listFields() {
      const list_header = this.headers.filter((x) => x.isFilter);
      return list_header.map((x) => ({
        text: x.text,
        value: x.value,
      }));
    },
  },
  mounted() {},
};
</script>
<style>
@keyframes price-change {
  from {
    background-color: #4caf50;
  }
  to {
    background-color: black;
  }
}
.changed {
  animation-name: price-change;
  animation-duration: 2s;
}
.normal {
  color: white;
}
.color-red {
  color: #f44336;
}
.color-green {
  color: #4caf50;
}
.color-orange {
  color: #ff9800;
}
.color-favorite {
  color: #ff9800;
}

.theme--dark.v-data-table > .v-data-table__wrapper > table > thead > tr > th {
  color: white;
}
.table-holder,
.table-responsive {
  position: relative;
}
.v-data-table > .v-data-table__wrapper > table > tbody > tr > td:first-child,
.v-data-table > .v-data-table__wrapper > table > tfoot > tr > td:first-child,
.v-data-table > .v-data-table__wrapper > table > thead > tr > td:first-child {
  font-size: 0.875rem;
  height: 48px;
  position: -webkit-sticky;
  position: sticky;
  left: 0;
}
.v-data-table tr > td:first-child {
  background-color: #1e1e1e;
  z-index: 2;
}
.v-data-table th:first-child {
  background-color: #1e1e1e;
  z-index: 2;
  position: -webkit-sticky;
  position: sticky;
  left: 0;
}
.v-data-table th {
  text-align: center;
  padding: 10px 0px;
}
.thead > tr > th:first-child {
  background-color: azure;
}
.v-data-table {
  background:url(@/assets/watermarked.png) no-repeat; 
  background-position: center;
}
</style>
