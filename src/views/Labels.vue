<template>
	<Layout>
		<div class="labels-wrapper">
			<ol class="labels" v-if=" isShow === '-'">
				<li v-for="(item, index) in outputTags"
						:key="index">
				<span class="iconItem">
					<Icon :name="item.svg"></Icon>
					<span>{{ item.name }}</span>
				</span>
					<Icon :name="'错误'" @click="deleteTag(item.id)"></Icon>
				</li>
			</ol>

			<ol class="labels" v-if=" isShow === '+'">
				<li v-for="(item, index) in inputTags"
						:key="index">
				<span class="iconItem">
					<Icon :name="item.svg"></Icon>
					<span>{{ item.name }}</span>
				</span>
					<Icon :name="'delete2'" @click="deleteTag(item.id)"
					></Icon>
				</li>
			</ol>
			<span class="button-wrapper" v-if=" isShow === '-'">
				<router-link to="/labels/edit">
					<Button>新建标签</Button>
				</router-link>
			</span>
			<span class="button-wrapper" v-if=" isShow === '+'">
				<router-link to="/labels/edit">
					<Button>新建标签</Button>
				</router-link>
			</span>

		</div>

	</Layout>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Watch} from 'vue-property-decorator';
  import Types from '@/components/money-group/Type.vue';
  import Button from '@/components/Button.vue';
  import initialRecord from '@/constants/initialRecord';
  import clone from '@/lib/clone';
  // import draggable from 'vuedraggable';


  @Component({
    components: {Button, Types},

  })
  export default class Labels extends Vue {


    beforeCreate() {
      this.$store.commit('fetchTags');

    }

    created() {
      this.$store.commit('getInTags');
      this.$store.commit('getOutTags');
    }

    tags = this.$store.state.tagList;
    outputTags = this.$store.state.outTags;
    inputTags = this.$store.state.inTags;


    record: RecordItem = clone(initialRecord);
    isShow!: string;

    @Watch('record.type', {immediate: true})
    onRecordTypeChange(value: string) {
      this.isShow = value;
    }


    @Watch('tags', {immediate: true})
    onTagChange(tags: Tag[]) {
      console.log(tags)
      this.outputTags = tags.filter((item: Tag) => item.type === '-');
      this.inputTags = tags.filter((item: Tag) => item.type === '+');
    }


    deleteTag(id: string) {
      this.$store.commit('removeTag', id);
    }


  }
</script>

<style scoped lang="scss">
.content{
  display:flex;

}
	.labels-wrapper::-webkit-scrollbar {
		display: none;
	}

	.labels-wrapper {
		display: flex;
		flex-direction: column;
	}

	.labels {
		display: flex;
		flex-wrap: nowrap;
		flex-direction: column;
		font-size: 16px;
		max-height: 65vh;

		li {
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			align-items: center;
			border-bottom: 1px solid #e6e6e6;
			padding: 4px 15px;

			> .iconItem {
				display: flex;
				align-items: center;
				padding: 5px 2px;

				& > span {
					font-size: 18px;
				}
			}

			> .iconItem svg {
				border: 1px solid #333;
				padding: 5px;
				width: 40px;
				height: 40px;
				border-radius: 50%;
				background: white;
				margin-right: 5px;
			}
		}
	}

	.button-wrapper {
		display: inline-block;
		margin: 50px auto;

	}
  

</style>
