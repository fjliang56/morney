<template>
  <Layout class-prefix="layout">
    <Tags @update:value="record.tags = $event"/>
    <div class="notes">
      <FormItem field-name="备注"
                placeholder="在这里输入备注"
                :value.sync="record.notes"
      />
    </div>
    <Tabs :data-source="recordTypeList"
          :value.sync="record.type"/>
    <Number-pad :value.sync="record.amount" @submit="saveRecord"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import {Component} from 'vue-property-decorator';
import NumberPad from '@/components/Money/NumberPad.vue';
import Tabs from '@/components/Tabs.vue';
import FormItem from '@/components/Money/FormItem.vue';
import Tags from '@/components/Money/Tags.vue';
import recordTypeList from '@/constants/recordTypeList';



@Component({
  components: {Tabs, Tags,FormItem, NumberPad},
})
export default class Money extends Vue {
  get recordList() {
    return this.$store.state.recordList;
  }

  recordTypeList = recordTypeList;

  record: RecordItem= {
    tags: [], notes: '', type: '-', amount: 0
  };

  created() {
    this.$store.commit('fetchRecords');
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  saveRecord() {
    if(!this.record.tags || this.record.tags.length === 0){
      return window.alert('请至少选择一个标签');
    }
    this.$store.commit('createRecord', this.record);
    this.record.notes = '';
  }
}
</script>

<style lang="scss" scoped>
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}

.notes {
  padding: 12px 0;
}
</style>