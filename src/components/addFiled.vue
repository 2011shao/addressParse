<template>
  <a-modal
    title="新增字段"
    :visible="props.modelValue"
    width="300px"
    ok-text="创建"
    @cancel="hidepop"
    @close="hidepop"
    @before-ok="commitVoid"
    @ok="hidepop"
    :body-style="{ padding: '10px' }"
  >
    <div class="grid-one grid-gap-5" style="max-height: 60vh">
      <a-checkbox-group v-model="checkFieldArr">
        <a-checkbox
          v-for="(item, index) in addressFieldArr"
          :key="index"
          :value="item.value"
          >{{ item.label }}</a-checkbox
        >
      </a-checkbox-group>
      <a-input
        v-for="(item, index) in addressFieldArr"
        :key="index"
        v-show="showHideField(item.value)"
        :placeholder="item.label"
        v-model="item.fileName"
      ></a-input>
    </div>
  </a-modal>
</template>
<script setup>
import { ref } from "vue";
const emit = defineEmits(["update:modelValue", "commit"]);
const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
});
const checkFieldArr = ref(["name", "phone", "ssq", "details"]);
const addressFieldArr = ref([
  { label: "收货人", value: "name", fileName: "收货人姓名" },
  { label: "联系电话", value: "phone", fileName: "收货人手机号" },
  { label: "省市区", value: "ssq", fileName: "省市区" },
  { label: "详情地址", value: "details", fileName: "详情地址" },
  { label: "省份", value: "shen", fileName: "省份" },
  { label: "城市", value: "shi", fileName: "城市" },
  { label: "区县", value: "qu", fileName: "区县" },
  { label: "邮编", value: "yb", fileName: "邮编" },
  { label: "省市区编号", value: "ssqbh", fileName: "省市区编号" },
]);
function commitVoid(done) {
  const arr = addressFieldArr.value.filter((a) =>
    checkFieldArr.value.includes(a["value"])
  );
  emit("commit", { arr, done });
}
function hidepop() {
  emit("update:modelValue", false);
}
function showHideField(fileName) {
  return checkFieldArr.value.includes(fileName);
}
</script>