<template>
	<view>
		<u-collapse align="left">
			<u-collapse-item :title="item.title" name="Docs guide" :isLink="false" v-for="(item, index) in list">
				<view slot="value" class="u-page__item__title__slot-title">
					<label>
						<checkbox :checked="item.checked" @click="listAll(item.title, index)" /><text></text>
					</label>
				</view>
				<view class="content">
					<label v-for="items in item.person" :style="itemStyle">
						<text>{{ items.username }}</text>
						<checkbox :checked="items.checked" @click="listItem(item.title, items.username, index)" />
					</label>
				</view>
			</u-collapse-item>
		</u-collapse>
	</view>
</template>

<script>
export default {
	name: "selection",
	props: {
		list: {
			type: Array,
			require: true
		},
		itemStyle: {
			type: Object
		}
	},
	data() {
		return {
			optuser: '',
			checkeds: [],
			optlist: '',
		}
	},
	create(){
       this.list.forEach(e=>this.checkeds.push([]))
	},
	methods: {
		listAll(department, index) {
			this.list.forEach(e => {
				if (e.title == department) {
					const result = e.person.every(i => i.checked)
					result ? e.person.forEach(i => {
						i.checked = !i.checked
						this.checkeds[index].pop()
					}) : e.person.forEach(i => {
						if (!i.checked) {
							i.checked = true
							this.checkeds[index].push(i)
						}
					})
					e.checked = !e.checked
				}
			})
		},
		listItem(department, item, index) {
			this.list.forEach(e => {
				if (e.title == department) {
					e.person.forEach(i => {
						if (i.username == item) {
							i.checked = !i.checked
							if (i.checked) {
								this.checkeds[index].push(i)
							} else {
								this.checkeds[index].filter((e, index, arr) => {
									if (e.username === item) {
										arr.splice(index, 1)
									}
								})
							}
						}
					})
					const result = e.person.every(e => e.checked)
					if (result) {
						e.checked = true
					} else {
						e.checked = false
					}
				}
			})
		},
	},
	watch: {
		checkeds: {
			handler(newV, oldV) {
				const res = newV.map(e => {
					return e.map(e => e.username)
				})
				const list = res.flat().join(',')
				this.optuser = list
				this.optlist = newV.flat()
				this.$emit("click", this.optlist, this.optuser)
			},
			deep: true
		}
	},
}
</script>

<style lang="scss">
.content {
	display: flex;
	flex-direction: column;
	gap: 10rpx;

	label {
		padding-left: 50rpx;
		display: flex;
		justify-content: space-between;
	}
}
</style>