<template>
  <div>
    <title-banner title="告警列表"/>
    <div class="catalog">
      <div v-for="ct in warningCategories" class="category" :key="ct">{{ct}}</div>
      <div class="img"><img src="../assets/target.png"/></div>
      <div class="img"><img src="../assets/clock.png"/></div>
    </div>
    <div id="warning-info">
      <div class="warning-table" id="warning-info-table">
        <div v-for="h in warningTableHeader" class="th" :key="h">{{h}}</div>
        <div v-for="r in warningTableContent" class="td" :key="r">{{r}}</div>
      </div>
      <div class="warning-table" id="op-txt-table">
        <div class="th">操作</div>
        <div v-for="doc in documents" class="td" :key="doc">
          <img src="../assets/document.png" :href="doc"/>
        </div>
      </div>
    </div>
    <div class="empty-flex-div"></div>
    <page-switching v-bind="pageSwtichingProps"/>
  </div>
</template>

<script>
import TitleBanner from './TitleBanner.vue';
export default {
  name: "WarningTable",
  setup: function() {
    return {
      pageSwtichingProps: {
        class: "page-switching",
        initialPage: 1,
        totalPage: 10,
        flex: "none",
        justifyContent: "flex-end",
        height: "30px",
        display: "flex",
        fontSize: "80%"
      },
      warningCategories: ["火警", "故障", "异常", "视频"],
      documents: Array(5).fill("#"),
      warningTableHeader: ["设备名称", "设备位置", "告警类别", "告警时间"],
      warningTableContent: ((loop) => {
        let _arr = [];
        for (let _i = 0; _i < loop; _i++) {
          _arr = [..._arr, "某某某某设备", "某某某某楼梯口", "火警", "2022-06-22"];
        }
        return _arr;
      })(5)
    };
  },
  components: {
    TitleBanner
  }
}
</script>

<style>
.catalog {
    display: flex;
    height: 40px;
    flex: none;
}
.catalog .category {
    font-size: 70%;
    border: 1px solid white;
    margin-left: 2px;;
    flex: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 5px;
}
.catalog .img {
    background-color: blue;
    margin-left: 5px;
    flex: initial;
    width: 40px;
    margin-bottom: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.catalog img {
    height: 70%;
}

#warning-info {
    display: flex;
    flex: none;
}

#warning-info .warning-table {
    display: grid;
    grid-auto-rows: 40px;
    text-align: center;
    font-size: 70%;
    white-space: nowrap;
}
#warning-info .warning-table .th,
#warning-info .warning-table .td {
  display: flex;
  align-items: center;
  justify-content: center;
}
#warning-info .warning-table .th {
    background-color: gray;
}
#warning-info .warning-table .td {
    margin-left: 5px;
}

#warning-info-table {
    flex: auto;
    grid-template-columns: repeat(4, 1fr);
    overflow-x: auto;
    overflow-y: hidden;
}
#op-txt-table {
    flex: none;
    justify-content: flex-end;
    width: 50px;
    grid-template-columns: 1fr;
    overflow: hidden;
    box-shadow: inset 10px 0px 10px rgba(255, 255, 255, 0.3);
}

#op-txt-table .td {
    display: flex;
    align-items: center;
    justify-content: center;
}
#op-txt-table .td img {
    height: 50%;
}

.empty-flex-div {
  flex: auto;
}
</style>
