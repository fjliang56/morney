<template>
  <Layout class-prefix="layout">
    <Tags :data-source.sync="tags" @update:value="onUpdateTags"/>
    <div class="notes">
      <FormItem field-name="备注"
           placeholder="在这里输入备注"
           @update:value="onUpdateNotes"/>
    </div>
    <Types :value.sync="record.type"/>
    <Number-pad :value.sync="record.amount" @submit="saveRecord"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import {Component} from 'vue-property-decorator';
import NumberPad from '@/components/Money/NumberPad.vue';
import Types from '@/components/Money/Types.vue';
import FormItem from '@/components/Money/FormItem.vue';
import Tags from '@/components/Money/Tags.vue';


@Component({
  components: {Tags, FormItem, Types, NumberPad}
})
export default class Money extends Vue {
  tags = window.tagList;
  recordList = window.recordList;
  record = {
    tags: [], notes: '', type: '-', amount: 0
  };

  onUpdateTags(value: string[]) {
    this.record.tags = value;
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  saveRecord() {
    window.createRecord(this.record);
  }

}
</script>

<style lang="scss" scoped>
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}
.notes{
  padding: 12px 0;
}
</style>