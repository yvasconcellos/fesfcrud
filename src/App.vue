<template>
  <div class="vh-100 d-flex flex-column">
    <HeaderCrud />
    <div class="main-input-form my-4">
      <input
        v-model="this.taskInput"
        type="text"
        name="inputFormTask"
        id="inputFormTask"
        placeholder="Tarefa..."
        class="form-control"
      />
      <button @click="addTask" class="btn btn-primary">Adicionar Tarefa</button>
    </div>
    <div class="table-responsive px-4">
      <table class="table table-hover align-middle">
        <thead class="table-light text-center">
          <tr>
            <th>Tarefa</th>
            <th>Status</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody class="text-center">
          <tr v-for="(task, index) in arrayTasks">
            <th class="fw-normal text-break w-50">{{ task.name }}</th>
            <th class="fw-normal w-25">{{ task.status }}</th>
            <th class="w-25">
              <button
                :value="index"
                @click="() => handleShowEditForm(index, task.name)"
                class="btn btn-secondary btn m-1"
                data-bs-toggle="modal"
                data-bs-target="#editModal"
              >
                Editar
              </button>
              <button
                :value="index"
                @click="removeTask"
                class="btn btn-danger btn m-1"
              >
                Apagar
              </button>
            </th>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Modal -->
    <div
      class="modal fade"
      id="editModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="editModalLabel">Editar Tarefa</h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body main-input-form">
            <input
              v-model="this.taskInputEdited"
              type="text"
              name="inputFormTaskEdited"
              id="inputFormTaskEdited"
              class="form-control w-50"
            />
            <select
              name="status"
              id="status"
              v-model="this.statusEdited"
              class="form-select w-50"
            >
              <option value="Concluído">Concluído</option>
              <option value="Em Andamento">Em Andamento</option>
              <option value="Aguardando Inicio">Aguardando Inicio</option>
            </select>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Fechar
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="editTask"
              data-bs-dismiss="modal"
            >
              Salvar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderCrud from "./components/HeaderCrud.vue";
export default {
  name: "App",

  mounted() {
    if (localStorage.getItem("tasks")) {
      this.arrayTasks = JSON.parse(localStorage.getItem("tasks"));
    } else {
      localStorage.setItem("tasks", []);
    }
  },

  data() {
    return {
      taskInput: "",
      arrayTasks: [],
      indexTaskToEdit: null,
      taskInputEdited: "",
      showEditForm: false,
      statusEdited: "Aguardando Inicio",
    };
  },

  methods: {
    addTask() {
      const dataTask = { name: this.taskInput, status: "Aguardando Inicio" };
      this.arrayTasks.push(dataTask);
      this.taskInput = "";
      localStorage.setItem("tasks", JSON.stringify(this.arrayTasks));
    },
    handleShowEditForm(index, nameTask) {
      this.showEditForm = !this.showEditForm;
      this.indexTaskToEdit = index;
      this.taskInputEdited = nameTask;
    },
    removeTask(e) {
      const {
        target: { value },
      } = e;
      this.arrayTasks.splice(value, 1);
      localStorage.setItem("tasks", JSON.stringify(this.arrayTasks));
    },
    editTask() {
      const dataTask = {
        name: this.taskInputEdited,
        status: this.statusEdited,
      };
      this.arrayTasks.splice(this.indexTaskToEdit, 1, dataTask);
      localStorage.setItem("tasks", JSON.stringify(this.arrayTasks));
      this.showEditForm = false;
    },
  },

  components: {
    HeaderCrud,
  },
};
</script>

<style>

.main-input-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

#inputFormTask {
  width: 50%;
}

@media (min-width: 576px) {
  .main-input-form {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }

  #inputFormTask {
  width: 25%;
}

}

</style>
