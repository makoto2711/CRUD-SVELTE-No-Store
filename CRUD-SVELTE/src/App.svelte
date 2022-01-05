<script>
	 import Swal from 'sweetalert2'
	import { Toast } from "bootstrap";

	let toastEl;
 
 



let opc = {texto: "", color:""}

let instancia
$: if (toastEl) 
{
	instancia = new Toast(toastEl)
}
 

const mostrarToast = (texto, color) => 
{
	opc = {texto, color} 
	instancia.show()
	/* 
	Swal.fire({
	icon: 'error', 
	text: 'Something went wrong!'
	}) */
}
 

	let todos = []
	let todo = {id: "", 
				texto:"", 
				estado: false};
 

	if (localStorage.getItem("todos")) {
		todos = JSON.parse(localStorage.getItem("todos"))	
	}			

	$: localStorage.setItem("todos", JSON.stringify(todos))



	const addTodo = ()=> 
	{
		
		if ( !todo.texto.trim() ) 
		{
			todo.texto = ""
			return
		}

		todo.id = Date.now()
		todos = [...todos, todo]
		console.log(todos);

		todo = {id: "", 
				texto:"", 
				estado: false}

		mostrarToast("Todo agregado", "bg-primary")
	}


	const delTodo = id => 
	{
		todos = todos.filter(item => item.id !== id)
		mostrarToast("Todo eliminado", "bg-danger")
	}


	const editTodo = id => 
	{
		todos = todos.map( item => 
			item.id === id ? 
			{...item, estado: !item.estado}
			: item
		)
		
		mostrarToast("Todo editado", "bg-warning")
	}

	const classIcono = valor => 
	(
		valor ? "bi bi-arrow-clockwise" : "bi bi-check2"
	)

	const classEstado = valor => valor ? "btn-success" : "btn-warning"

</script>
 

<div class="container">
	<h1 class="display-5" >CRUD</h1>

	<form on:submit|preventDefault={addTodo}  >
		<input type="text"
			placeholder="Enter para agregar todo"
			class="form-control"
			bind:value={todo.texto}
		>

	</form>


 

	{#each todos as item}
		<div class="shadow my-3 p-3 lead">
			<p
			class={ item.estado ? "text-decoration-line-through":"" }
			>{item.texto} </p>

			<button
				on:click={editTodo(item.id)}
				class="btn btn-sm {classEstado(item.estado)}">
			<i class={classIcono(item.estado)}></i>	
			</button>

			<button
				on:click={delTodo(item.id)}
				class="btn btn-sm btn-danger">
			<i class="bi bi-trash"></i>
			</button>
		</div>
	{/each}

	<div class="toast-container position-absolute p-3 top-0 end-0">

		<div  bind:this={toastEl} class="toast align-items-center text-white {opc.color} border-0" role="alert" aria-live="assertive" aria-atomic="true">
			<div class="d-flex">
			  <div class="toast-body">
				{opc.texto}
			  </div>
			  <button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast" aria-label="Close"></button>
			</div>
		</div>

	</div>

	


</div>

 