<template>
    <div>
        <form @submit.prevent="agregar()" v-if="editarActivo">
            <h3>Editar Nota</h3>
            <input
                type="text"
                placeholder="Nombre"
                class="form-control mb-2"
                v-model="nota.nombre"
            />
            <input
                type="text"
                placeholder="Descripción"
                class="form-control mb-2"
                v-model="nota.descripcion"
            />
            <button class="btn btn-success btn-sm">Guardar</button>
        </form>

        <form @submit.prevent="agregar()" v-else>
            <h3>Agregar Nota</h3>
            <input
                type="text"
                placeholder="Nombre"
                class="form-control mb-2"
                v-model="nota.nombre"
            />
            <input
                type="text"
                placeholder="Descripción"
                class="form-control mb-2"
                v-model="nota.descripcion"
            />
            <button class="btn btn-primary btn-sm">Agregar</button>
        </form>

        <hr class="mt-3" />
        <h3>Lista de notas:</h3>
        <ul class="list-group my-2">
            <li
                class="list-group-item"
                v-for="(item, index) in notas"
                :key="index"
            >
                <span class="badge badge-primary float-right">
                    <!-- {{ item.updated_at }} -->
                </span>
                <p>{{ item.nombre }}</p>
                <p>{{ item.descripcion }}</p>
                <button
                    class="btn btn-danger btn-sm"
                    @click="eliminarNota(item)"
                >
                    Eliminar
                </button>
                <button
                    class="btn btn-warning btn-sm"
                    @click="editarNota(item)"
                >
                    Editar
                </button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            notas: [],
            nota: { nombre: "", descripcion: "" },
            editarActivo: false,
            val: {
                empty: "Debes completar todos los campos antes de guardar"
            }
        };
    },
    async created() {
        const me = this;
        await me.getNotes();
    },
    methods: {
        async getNotes() {
            const me = this;
            const { data } = await axios.get("/notas");
            me.notas = await data;
        },
        async agregar() {
            const me = this;
            const nuevaNota = me.nota;
            const { nombre, descripcion } = nuevaNota;
            if (nombre.trim() === "" || descripcion.trim() === "") {
                alert(me.val.empty);
                return;
            }
            me.nota = { nombre: "", descripcion: "" };

            const response = await axios.post("/notas", nuevaNota);
            if (response) {
                me.notas.push(response.data);
            }
        },
        async eliminarNota(item, index) {
            const me = this;
            const response = await axios.delete(`/notas/${item.id}`);
            if (response) {
                me.notas.splice(index, 1);
            }
        },
        editarNota(nota) {
            const me = this;
            me.editarActivo = true;
            me.nota.nombre = nota.nombre;
            me.nota.descripcion = nota.descripcion;
        }
    }
};
</script>
