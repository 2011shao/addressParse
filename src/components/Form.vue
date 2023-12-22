
<template>
  <div>
     <a-typography-text>点击→
    <a-typography-text type="primary" @click="helpVoid">
      查看教程
    </a-typography-text>
       </a-typography-text>

    
  
  <a-spin :loading="loading" style="width: 100%;">
    <div class="grid-one p-all-1 grid-gap-5">
      <a-typography-title :heading="6" style="">
        选择需要解析的地址
      </a-typography-title>
      <a-select
        allow-clear
        v-model="origin_filed"
        placeholder="选择地址字段"
        :options="getSelectOptions(0)"
        :field-names="{ value: 'id', label: 'name' }"
      ></a-select>
      <div class="row-between-center">
        <a-typography-title :heading="6" class="flex-grow">
          解析后存放字段
        </a-typography-title>
        <a-tooltip content="新增目标字段">
          <a-button status="success" type="text" @click="showAddPop">
            <icon-plus-circle size="20" />
            新增
          </a-button>
        </a-tooltip>
        <a-popover trigger="click" position="lt">
          <icon-settings size="20" />
          <template #content>
            <a-checkbox-group v-model="checkFieldArr" direction="vertical">
              <a-checkbox
                v-for="(item, index) in addressFieldArr"
                :key="index"
                :value="item.value"
                >{{ item.label }}</a-checkbox
              >
            </a-checkbox-group>
          </template>
        </a-popover>
      </div>
      <div class="row-start-center" v-show="showHideField('name')">
        <a-typography-text class="labelCss">收货人</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.name"
          placeholder="收货人姓名"
          :options="getSelectOptions(1)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.name = '')"
        >
        </a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('phone')">
        <a-typography-text class="labelCss">联系电话</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.phone"
          placeholder="收货人手机号"
          :options="getSelectOptions(2, [1, 13])"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.phone = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('ssq')">
        <a-typography-text class="labelCss">省市区</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.ssq"
          placeholder="省市区"
          :options="getSelectOptions(3)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.ssq = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('details')">
        <a-typography-text class="labelCss">详细地址</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.details"
          placeholder="详细地址"
          :options="getSelectOptions(4)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.details = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('shen')">
        <a-typography-text class="labelCss">省份</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.shen"
          placeholder="省份"
          :options="getSelectOptions(7)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.shen = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('shi')">
        <a-typography-text class="labelCss">城市</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.shi"
          placeholder="城市"
          :options="getSelectOptions(8)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.shi = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('qu')">
        <a-typography-text class="labelCss">区县</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.qu"
          placeholder="区县"
          :options="getSelectOptions(9)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.qu = '')"
        ></a-select>
      </div>

      <div class="row-start-center" v-show="showHideField('yb')">
        <a-typography-text class="labelCss">邮编</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.yb"
          placeholder="邮编"
          :options="getSelectOptions(5)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.yb = '')"
        ></a-select>
      </div>
      <div class="row-start-center" v-show="showHideField('ssqbh')">
        <a-typography-text class="labelCss">省市区编号</a-typography-text>
        <icon-double-right />
        <a-select
          class="flex-grow"
          allow-clear
          v-model="target_dic.ssqbh"
          placeholder="省市区编号"
          :options="getSelectOptions(6)"
          :field-names="{ value: 'id', label: 'name' }"
          @clear="() => (target_dic.ssqbh = '')"
        ></a-select>
      </div>
      <a-button
        :loading="buttonLoading"
        :disabled="!commitCan"
        type="primary"
        @click="exportVoid"
        >开始转换</a-button
      >
      <a-progress v-if="buttonLoading" :percent="progress" />
    </div>
    <AddFiledView v-model="showPop" @commit="addAddressField"></AddFiledView>
  </a-spin>
    </div>
</template>

<script setup >
import { bitable, FieldType } from "@lark-base-open/js-sdk";
import { ref, onMounted, computed, watch, onUnmounted } from "vue";
import dayjs from "dayjs";
import { Message } from "@arco-design/web-vue";
import AddressParse from "./address/parse/index";
import { cloneDeep } from "lodash";
import AddFiledView from "./addFiled.vue";
const buttonLoading = ref(false);
const showPop = ref(false);
const loading = ref(false);
const fieldList = ref([]);
const origin_filed = ref("");
const progress = ref(0);
const target_dic = ref({
  name: "",
  phone: "",
  ssq: "",
  details: "",
  yb: "",
  ssqbh: "",
  shen: "",
  shi: "",
  qu: "",
});
const addressFieldArr = ref([
  { label: "收货人", value: "name" },
  { label: "联系电话", value: "phone" },
  { label: "省市区", value: "ssq" },
  { label: "详细地址", value: "details" },
  { label: "省份", value: "shen" },
  { label: "城市", value: "shi" },
  { label: "区县", value: "qu" },
  { label: "邮编", value: "yb" },
  { label: "省市区编号", value: "ssqbh" },
]);
const checkFieldArr = ref(["name", "phone", "ssq", "details"]);
let table = "";
onMounted(async () => {
  initBaeData();
  watchSwitchTable();
});
async function watchSwitchTable() {
  bitable.base.onSelectionChange((event) => {
    initBaeData();
  });
}
async function initBaeData() {
  loading.value = true;
  table = await bitable.base.getActiveTable();
  getAllField(true);
}
async function getAllField(loadCache = false) {
  loading.value = true;
  const fieldMetaList = await table.getFieldMetaList();
  fieldList.value = fieldMetaList;
  loading.value = false;
  const addressFiled = fieldList.value.find(
    (a) => a["name"].includes("地址") && a["type"] == 1
  );

  if (loadCache) {
    const cacheDicStr = localStorage.getItem(table.id);
    if (cacheDicStr) {
      const cacheDic = JSON.parse(cacheDicStr);
      checkFieldArr.value = cacheDic.checkFieldArr;
      for (let key of Object.keys(cacheDic.target_dic)) {
        const addressFiled = fieldList.value.find(
          (a) => a["id"] == cacheDic.target_dic[key]
        );
        if (addressFiled) {
          target_dic.value[key] = addressFiled["id"];
        }
      }
      origin_filed.value = cacheDic.origin_filed;
    } else {
      checkFieldArr.value = ["name", "phone", "ssq", "details"];
      target_dic.value = {
        name: "",
        phone: "",
        ssq: "",
        details: "",
        yb: "",
        ssqbh: "",
        shen: "",
        shi: "",
        qu: "",
      };
    }
  }
  if (addressFiled) {
    origin_filed.value = addressFiled["id"];
  }
}

function showAddPop() {
  showPop.value = true;
}
async function addAddressField({ arr, done }) {
  let fileId = "";
  for (let item of arr) {
    const czItem = fieldList.value.find((a) => a["name"] == item["fileName"]);
    if (czItem) {
      fileId = czItem["id"];
    } else {
      fileId = await table.addField({
        type: FieldType.Text,
        name: item["fileName"],
      });
    }
    target_dic.value[item["value"]] = fileId;
  }
  getAllField();
  done(true);
}

// 导出word
async function exportVoid() {
  localStorage.setItem(
    table.id,
    JSON.stringify({
      checkFieldArr: checkFieldArr.value,
      target_dic: target_dic.value,
      origin_filed: origin_filed.value,
    })
  );
  progress.value = 0;
  buttonLoading.value = true;
  loading.value = true;
  const recordList = await table.getRecordList();
  const view = await table.getActiveView();
  const recordIdList = await view.getVisibleRecordIdList();
  let newDataArr = [];
  let haveFalse = false;
  let i = 0;
  for (const record of recordList) {
    if (!recordIdList.includes(record.id)) {
      continue;
    }
    // const cellList = await record.getCellList();
    const cell = await record.getCellByField(origin_filed.value);
    const value = cell.val;
    if (!value) {
      continue;
    }
    let valueStr = "";
    if (Array.isArray(value)) {
      valueStr = value.map((a) => a["text"]).join("");
    } else if (typeof value == "object") {
      valueStr = value["text"];
    } else {
      valueStr = value;
    }
    const [result] = AddressParse.parse(valueStr);
    if (result) {
      let phoneStr=result.mobile||result.phone
      if(result.phone && result.mobile)
      {
        phoneStr=[result.mobile, result.phone].join(",")
      }
      const dic = {
        recordId: record.id,
        fields: {
          [target_dic.value.name]: result.name,
          [target_dic.value.phone]: phoneStr,
          [target_dic.value
            .ssq]: `${result.province}${result.city}${result.area}`,
          [target_dic.value.details]: result.details,
          [target_dic.value.yb]: result.zip_code,
          [target_dic.value.ssqbh]: result.code,
          [target_dic.value.shen]: result.province,
          [target_dic.value.shi]: result.city,
          [target_dic.value.qu]: result.area,
        },
      };

      delete dic["fields"][""];
      newDataArr.push(dic);
    } else {
      haveFalse = true;
    }
    i++;
    progress.value =parseInt(i / recordIdList.length);
  }
  if (haveFalse) {
    Message.info("部分数据异常已跳过处理");
  }
  await table.setRecords(newDataArr);
  buttonLoading.value = false;
  loading.value = false;
}

function getSelectOptions(selfIndex, canTypeArr = [1]) {
  let arr = [];
  const selectId = Object.values(target_dic.value);
  selectId.splice(selfIndex, 1);
  const allFiled = cloneDeep(fieldList.value);
  for (let item of allFiled) {
    if (canTypeArr.includes(item["type"])) {
      item["disabled"] = false;
      if (selectId.includes(item["id"])) {
        item["disabled"] = true;
      }
      arr.push(item);
    }
  }
  return arr;
}
const commitCan = computed(() => {
  const selectId = Object.values(target_dic.value);
  const noEmpty = selectId.find((a) => a);
  if (origin_filed.value && noEmpty) {
    return true;
  }
  return false;
});

function showHideField(fileName) {
  return checkFieldArr.value.includes(fileName);
}
  function helpVoid(params) {
    window.open('https://y35xdslz6g.feishu.cn/docx/QB5udpPFgotthpxKRaJcSaTmnOM?from=from_copylink','_blank')
  }
</script>
<style >
.labelCss {
  width: 70px;
  text-align: center;
  flex-shrink: 0;
}
</style>
