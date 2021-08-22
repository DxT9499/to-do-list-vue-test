<template>
	<div class="new-task">
		<h5>New Task</h5>
		<input
			class="task-name"
			type="text"
			v-model="name"
			placeholder="Add new tasks..."
		/>
		<label class="discription">Description</label>
		<textarea v-model="discription"></textarea>
		<div class="side-by-side">
			<div class="side">
				<label>Due Date</label>
				<input type="date" v-model="dueDate" v-bind:min="minDay" />
			</div>
			<div class="side">
				<label>Piority</label>
				<select v-model="piority">
					<option>Low</option>
					<option>Normal</option>
					<option>High</option>
				</select>
			</div>
		</div>
		<button id="add-btn" @click="sendNewItemData">Add</button>
	</div>
</template>

<script>
import { EventBus } from "./event-bus.js";

export default {
	name: "NewTask",
	components: {},
	data() {
		return {
			minDay: "",
			name: "",
			discription: "",
			dueDate: "",
			piority: "Normal",
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
		this.dueDate = `${year}-${month}-${day}`;
	},
	methods: {
		sendNewItemData() {
			if (this.name) {
				let newItem = {
					id : Math.random().toString(36).slice(2),
					isDone: false,
					name: this.name,
					discription: this.discription,
					dueDate: this.dueDate,
					piority: this.piority,
				};
				EventBus.$emit("new-item-data", newItem);
				this.name = "";
				this.discription = "";
				this.dueDate = this.minDay;
				this.piority = "Normal";
			} else alert("Please enter the name of task!!!");
		},
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.new-task * {
	margin-top: 10px;
	margin-bottom: 10px;
}
.new-task {
	border: 1px solid black;
}

h5 {
	position: relative;
	display: block;
	margin-top: 20px !important;
	margin-bottom: 20px !important;
}
.task-name {
	position: relative;
	display: block;
	margin-top: 10px;
	margin-bottom: 10px;
	width: 80%;
	height: 30px;
	margin: auto;
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
#add-btn {
	position: relative;
	display: block;
	margin-top: 20pxpx;
	border: none;
	color: white;
	margin: auto;
	width: 80%;
	height: 30px;
	background-color: #4daf50;
	margin-bottom: 10px;
	border-radius: 5px;
	cursor: pointer;
}
</style>
