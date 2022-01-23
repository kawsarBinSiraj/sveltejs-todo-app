<script>
	import { toasts, ToastContainer, FlatToast } from 'svelte-toasts';

	let todo = '';
	let todoList = [];
	let isEdit = false;
	let editId = null;

	function addToList() {
		if (todo.length < 1) return alert('Write Something First !!');
		if (isEdit === false) {
			todoList = [...todoList, { text: todo, isDone: false, id: new Date().getTime() }];
			todo = '';
			toasts.success('Successfully Added', { duration: 2500 });
		} else {
			updateTodo();
		}
	}

	function removeFromList(index) {
		todoList.splice(index, 1);
		todoList = todoList;
		toasts.success(`Successfully Removed `, { duration: 2500 });
	}
	function editFromList(text, id) {
		isEdit = true;
		todo = text;
		editId = id;
	}

	function updateTodo() {
		let newTodoList = todoList.map((t, i) => {
			if (editId === t.id) {
				return {
					...t,
					text: todo,
				};
			} else {
				return {
					...t,
				};
			}
		});

		todoList = newTodoList;
		isEdit = false;
		todo = '';
		editId = null;
		toasts.success(`Successfully Updated `, { duration: 2500 });
	}

	function doneFromList(id) {
		let newTodoList = todoList.map((t, i) => {
			if (id === t.id) {
				return {
					...t,
					isDone: !t.isDone,
				};
			} else {
				return {
					...t,
				};
			}
		});
		todoList = newTodoList;
	}
</script>

<main>
	<ToastContainer placement="bottom-center" let:data>
		<FlatToast {data} />
	</ToastContainer>
	<div class="container py-5">
		<div class=" row">
			<div class="col-xl-6 mx-auto">
				<h4 class="mb-2 fw-normal">Todo App By Svelt.js</h4>
				<img src="https://svelte.dev/svelte-logo-horizontal.svg" class="mb-3 img-fluid" alt="logo" />
				<h5 class="mb-2 fw-normal">Task Length : {todoList.length}</h5>
				<div class="todo-app d-flex align-items-center gap-1">
					<input type="text" bind:value={todo} class="form-control" placeholder="Write Here..." />
					<button on:click={addToList} class={`btn text-light px-3 ${isEdit ? 'btn-primary' : 'btn-warning'}`}>
						{isEdit ? 'Update' : 'Add'}
					</button>
				</div>
				<div class="todos-view mt-3">
					{#each todoList as { text, id, isDone }, index (index)}
						<div current={id} class="todo-item py-1 border-bottom d-flex align-items-center justify-content-between gap-3">
							<p style={`${isDone ? 'text-decoration : line-through' : ''}`} class={`mb-0`}>{text}</p>
							<div class="todo-action">
								<button on:click={() => editFromList(text, id)} type="button" class="btn btn-sm text-light mb-0 p-1 btn-info">Edit</button>
								<button on:click={() => removeFromList(index)} class="btn btn-sm p-1 btn-danger mb-0">Delete</button>
								<button on:click={() => doneFromList(id)} class="btn btn-sm p-1 btn-success mb-0">Done</button>
							</div>
						</div>
					{:else}
						<p class="mb-0 text-primary">No tasks today! &#128526;</p>
					{/each}
				</div>
			</div>
		</div>
	</div>
</main>
