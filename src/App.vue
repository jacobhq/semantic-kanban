<template>
	<semantic-kanban
		:tasks="tasks"
		:members="members"
		:options="options"
		@updateTask="updateTask"
		@deleteTask="deleteTask"
		@addTag="addTag"
		@deleteTag="deleteTag"
		@updateMember="updateMember">
	</semantic-kanban>  
</template>

<script>
import _ from 'lodash'
import SemanticKanban from './semanticKanban'

export default {
	name: 'app',
	components: {
		SemanticKanban
	},
	data () {
		return {
			tasks: [{
				id: 1,
				subject: 'Welcome to my kanban',
				description: 'This is a great todo list for me only because I am such a productivity freak.',
				status: 'backlog',
				owner: 1,
				tags: ['help-wanted'],
				dueDate: 'No due date'
			}
			],
			members: [{
				id: 1,
				name: 'Jacob'
			}],
			options: {
				defaultTaskDialog: true,
				defaultConfirmDialog: true,
				defaultMemberDialog: true,
				taskExtraContent () {
					return function () {
						var dueDate = this.task.dueDate
						const remain = 10
						const icon = this.task.type === 'feature'
							? 'blue cubes icon'
							: this.task.type === 'bug' ? 'red bug icon' : ''
						return `
							<div class="right floated meta">
								<span>Rem. ${remain}h</span>
								<i class="${icon}"></i>
							</div>
							${dueDate}`
					}
				}
			}
		}
	},
	methods: {
		updateTask (task) {
			const existingTask = _.find(this.tasks, {id: task.id})
			if (existingTask) {
				_.assign(existingTask, task)
			} else {
				task.id = _.last(_.sortBy(this.tasks, ['id'])).id + 1
				this.tasks.push(task)
			}
		},
		deleteTask (task) {
			_.remove(this.tasks, t => t.id === task.id)
		},
		deleteTag (task, tag) {
			console.log(task, tag)
		},
		addTag (task, tag) {
			const existingTask = _.find(this.tasks, {id: task.id})
			if (existingTask) {
				existingTask.tags.push(tag)
			}
		},
		updateMember (member) {
			const existingMember = _.find(this.members, {id: member.id})
			if (existingMember) {
				_.assign(existingMember, member)
			} else {
				member.id = _.last(_.sortBy(this.members, ['id'])).id + 1
				this.members.push(member)
			}
		}
	}
}
</script>
