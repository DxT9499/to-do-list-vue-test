<template>
	<div class="to-do-list">
		<h5>To Do List</h5>
		<SearchAutocomplete :items="items" @change-render-list="handleRenderList"></SearchAutocomplete>
		<ul>
			<Item
				v-for="render in renderList"
				v-bind:key="render.id"
                :itemId="render.id"
				:isDone="render.isDone"
				:name="render.name"
				:discription="render.discription"
				:dueDate="render.dueDate"
				:piority="render.piority"
				@update-data="handleUpdate"
				@delete-item="handleDeleteItem(render)"
				@is-done-changed="handleIsDone(render)"
			></Item>
		</ul>
		<div class="bulk-box" v-if="isShowBulkBox">
			<label>
				Bulk Action:
			</label>
			<button id="done-btn">Done</button>
			<button id="remove-btn" @click="handleRemoveWithBulk">
				Remove
			</button>
		</div>
	</div>
</template>

<script>
import Item from "./Item.vue";
import { EventBus } from "./event-bus.js";
import SearchAutocomplete from "./SearchAutocomplete.vue";
const LOCAL_STORAGE_KEY = "to-do-list";
export default {
	name: "ToDoList",
	components: {
		Item,
		SearchAutocomplete,
	},
	data() {
		return {
			items: JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY)) || [],
			isShowBulkBox: false,
            renderList: []
		};
	},
	created() {
        this.renderList = this.items
		this.isShowBulkBox = this.items.some((item) => {
			return item.isDone;
		});
	},
	methods: {
		handleUpdate(data) {
			// const index = this.items.indexOf(item);
			// console.log("featch data", index);
			// EventBus.$on("update-data", (data) => {
			// 	console.log("du lieu nhan dc", data);
			// 	this.items[index].name = data.newName;
			// 	this.items[index].discription = data.newDiscription;
			// 	this.items[index].dueDate = data.newDueDate;
			// 	this.items[index].piority = data.newPiority;
			// 	console.log("aray sau khi thay đổi", this.items);
			// });
            console.log(data.id);
            const index = this.items.indexOf(
                this.items.find(item => {
                    return item.id === data.id
                })
            );
            this.items[index].name = data.newName;
            this.items[index].discription = data.newDiscription;
            this.items[index].dueDate = data.newDueDate;
            this.items[index].piority = data.newPiority;
            console.log(this.items)
		},
		handleDeleteItem(item) {
			const index = this.items.indexOf(item);
			this.items.splice(index, 1);
            this.renderList = this.items
			if (
				this.items.length === 0 ||
				!this.items.some((item) => {
					return item.isDone;
				})
			)
				this.isShowBulkBox = false;
		},
		handleIsDone(item) {
			const index = this.items.indexOf(item);
			this.items[index].isDone = !this.items[index].isDone;
			this.isShowBulkBox = this.items.some(
				(item) => item.isDone === true
			);
		},
		handleRemoveWithBulk() {
			for (let i = 0; i < this.items.length; i++) {
				if (this.items[i].isDone) {
					this.handleDeleteItem(this.items[i]);
					i--;
					console.log(this.items.length);
				}
			}
			this.isShowBulkBox = false;
		},
        handleRenderList(results){
            console.log('come',results)
            this.renderList = results
        }
	},
	computed: {},
	watch: {
		items: {
			deep: true,
			handler(newValue) {
				localStorage.setItem(
					LOCAL_STORAGE_KEY,
					JSON.stringify(newValue)
				);
			},
		},
	},
	mounted() {
		EventBus.$on("new-item-data", (itemData) => {
			this.items.push(itemData);
			this.items.sort((a, b) => {
				return (
					Number(a.dueDate.split("-").join("")) -
					Number(b.dueDate.split("-").join(""))
				);
			});
		});
	},
};
</script>

<style scoped>
h5 {
	position: relative;
	display: block;
	margin-top: 20px !important;
	margin-bottom: 20px !important;
}
.to-do-list {
	position: relative;
	display: block;
	border: 1px solid black;
}
.to-do-list * {
	margin-top: 10px;
	margin-bottom: 10px;
}
.search-box {
	position: relative;
	height: 30px;
	width: 80%;
	margin-left: 10%;
	margin-top: 0px !important;
}
ul {
	width: 80%;
	padding-left: 0%;
	margin: auto;
	margin-bottom: 50px;
}
li {
	list-style-type: none;
	border: 1px solid black;
	width: 100%;
	height: 50px;
	margin: auto;
	vertical-align: middle;
}
button {
	border: none;
	color: white;
	width: 100px;
	height: 30px;
	border-radius: 5px;
	margin: auto;
	cursor: pointer;
}
#done-btn {
	background-color: #04bcd4;
}
#remove-btn {
	background-color: #d9534f;
}
.bulk-box {
	display: grid;
	position: absolute;
	grid-template-columns: 6fr 2fr 2fr;
	width: 100%;
	height: 50px;
	border: 1px solid black;
	background-color: #e0e0e0;
	vertical-align: middle;
	bottom: 0px;
	margin-bottom: 0px !important;
	vertical-align: middle;
}
.bulk-box label {
	margin-left: 10%;
	line-height: 30px;
}
</style>
