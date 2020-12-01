<template>
  <Layout class-prefix="money">
    <NumberPad @update:value="onUpdateAmount" @submit="saveRecord"/>
    <Type :value.sync="recordItem.type" />

    <div class="createdAt">
			<FormItem field-name="日期" placeholder="在这里输入日期"
								:value.sync="recordItem.createdAt"
								type="date"></FormItem>
		</div>
    <FormItem field-name="备注"
								:value="recordItem.note"
								@update:value="onUpdateNote"
								placeholder="写点备注吧..."
								is-number-pad="+"/>
   
  
   <Tagss  :fuckTags="fuckTags" @update:fucktags="onFuckChange"
					:is-show="recordItem.type"/>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
 type Tag = {
  id: string;
  svg: string;
  name: string;
  type: string;
}
type RecordItem = {
  type: string;
  tag: Tag;
  amount: string;
  note: string;
  createdAt?: string;
}

import Tagss from '@/components/money-group/Tagss.vue';
import NumberPad from '@/components/money-group/NumberPad.vue';
import FormItem from '@/components/money-group/FormItem.vue';
import Type from '@/components/money-group/Type.vue';
import initialRecord from '@/constants/initialRecord';
import clone from '@/lib/clone';

import {Component} from 'vue-property-decorator';

@Component({
    components: {Type, Tagss, NumberPad, FormItem},
  })

export default class Money extends Vue  {

fuckTags: Tag[] = [{id: '1', svg: 'meal', name: '餐饮', type: '-'}];

recordItem: RecordItem = clone(initialRecord);

 created() {
      this.$store.commit('fetchRecords');
    }
 onFuckChange(value: Tag[]) {
      this.fuckTags = value;
      this.recordItem.tag = value[0];
    }
  onUpdateAmount(value: string) {
      this.recordItem.amount = value;
    }
  onUpdateNote(value: string) {
      this.recordItem.note = value;
    }
  saveRecord() {
      if (this.recordItem.amount === '000') {
        window.alert('请填写金额');
        return;
      }
      this.$store.commit('createRecord', this.recordItem);
      this.clearData();
    }
      clearData() {
      this.recordItem = {
        type: this.recordItem.type, tag: this.recordItem.tag, amount: '0.00', note: '',
      };

    }

}
</script>

<style lang="scss">
  // ::v-deep .money-content {
  //   display: flex;
  //   flex-direction: column-reverse;
  //   border:1px solid red
  // }
  .createdAt {
		::v-deep .notes-wrapper {
			& input[type="date"] {
				position: relative;
			}

			& input[type="date"]::-webkit-datetime-edit {
				position: absolute;
				left: 30px
			}

			& input[type="date"]::-webkit-calendar-picker-indicator {
				position: absolute;
				width: 100%;
				left: -24px;
				top: 6px;

			}
		}
	}
</style>
