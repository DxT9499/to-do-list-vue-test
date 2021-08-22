<template>
	<div class="item">
		<div class="item-title">
			<input
				type="checkbox"
				v-model="isDoneInput"
				@change="handleIsDone"
			/>
			<label>{{ name }}</label>
			<button id="detail-btn" @click="isShowDetail = !isShowDetail">
				Detail
			</button>
			<button id="remove-btn" @click="deleteItem">Remove</button>
		</div>

		<div class="detail-info" v-if="isShowDetail">
			<input class="task-name" type="text" v-model="nameInput" />
			<label class="discription">Description</label>
			<textarea v-model="discriptionInput"></textarea>
			<div class="side-by-side">
				<div class="side">
					<label>Due Date</label>
					<input
						type="date"
						v-model="dueDateInput"
						v-bind:min="minDay"
					/>
				</div>
				<div class="side">
					<label>Piority</label>
					<select v-model="piorityInput">
						<option>Low</option>
						<option>Normal</option>
						<option>High</option>
					</select>
				</div>
			</div>
			<button id="update-btn" @click="updateItemData">Update</button>
		</div>
	</div>
</template>

<script>
// import { EventBus } from "./event-bus.js";
export default {
	props: {
        itemId: String,
		isDone: Boolean,
		name: String,
		discription: String,
		dueDate: String,
		piority: String,
	},
	data() {
		return {
			isShowDetail: false,
			updateData: {
                id: this.itemId,
				isDone: false,
				newName: this.name,
				newDiscription: this.discription,
				newDueDate: this.dueDate,
				newPiority: this.piority,
			},
		};
	},
	created() {
		var today = new Date();
		var month =
				today.getMonth() < 9
					? `0${today.getMonth() + 1}`
					: today.getMonth() + 1,
			day =
				today.getDate() < 10 ? `0${today.getDate()}` : today.getDate(),
			year = today.getFullYear();
		this.minDay = `${year}-${month}-${day}`;

	},
	computed: {
		nameInput: {
			get: function() {
				console.log(this.name);
				return this.name;
			},
			set: function(newValue) {
				console.log(newValue);
				this.updateData.newName = newValue;
				this.$emit("update:name", newValue);
				return newValue;
			},
		},
		discriptionInput: {
			get: function() {
				return this.discription;
			},
			set: function(newValue) {
                this.updateData.newDiscription = newValue;
				this.$emit("update:discription", newValue);
			},
		},
		dueDateInput: {
			get: function() {
				return this.dueDate;
			},
			set: function(newValue) {
                this.updateData.newDueDate = newValue;
				this.$emit("update:dueDate", newValue);
			},
		},
		piorityInput: {
			get: function() {
				return this.piority;
			},
			set: function(newValue) {
                this.updateData.newPiority = newValue;
				this.$emit("update:piority", newValue);
			},
		},
		isDoneInput: {
			get: function() {
				return this.isDone;
			},
			set: function(newValue) {
                this.updateData.newIsDone = newValue;
				this.$emit("update:isDone", newValue);
			},
		},
	},
	methods: {
		updateItemData() {
            this.$emit("update-data", this.updateData);
		},
		deleteItem() {
			this.$emit("delete-item");
		},
		handleIsDone() {
			this.$emit("is-done-changed");
		},
	},
};
</script>

<style scoped>
.item {
	position: relative;
	width: 100%;
	height: fit-content;
	border: 1px solid black;
	vertical-align: middle;
}
.item-title {
	display: grid;
	height: 50px;
	grid-template-columns: 1fr 5fr 2fr 2fr;
}
.item-title input[type="checkbox"] {
	margin: auto;
	height: 25px;
	height: 25px;
	cursor: pointer;
}
.item-title label {
	line-height: 50px;
}
.detail-info {
	width: 100%;
	display: block;
	border: 1px solid black;
}
.detail-info * {
	margin-top: 10px;
	margin-bottom: 10px;
}
.detail-info .task-name {
	position: relative;
	width: 80%;
	height: 30px;
	margin-left: 10%;
}
.discription {
	position: relative;
	display: block;
	margin-top: 10px;
	margin-bottom: 10px;
	margin-left: 10%;
}
textarea {
	position: relative;
	display: block;
	margin-top: 10px;
	margin-bottom: 10px;
	height: 200px;
}
.side-by-side {
	position: relative;
	margin-top: 10px;
	margin-bottom: 10px;
	display: grid;
	grid-template-columns: 1fr 1fr;
	width: 80%;
	margin: auto;
}
.side input {
	display: block;
	width: 90%;
	height: 26px;
}
.side-by-side > div:nth-child(2) label {
	margin-left: 10%;
}
.side select {
	margin-left: 10%;
	display: block;
	width: 90%;
	height: 30px;
}
button {
	border: none;
	border-radius: 5px;
	margin: auto;
	color: white;
	width: 90%;
	font-size: 1vw;
	height: 30px;
	cursor: pointer;
}
#detail-btn {
	width: 90%;
	font-size: 1vw;
	background-color: #04bcd4;
}
#remove-btn {
	width: 90%;
	vertical-align: middle;
	font-size: 1vw;
	background-color: #d9534f;
}
#update-btn {
	width: 80%;
	font-size: 1vw;
	background-color: #4daf50;
	margin-left: 10%;
	margin-bottom: 20px !important;
	margin-top: 20px !important;
}
</style>
