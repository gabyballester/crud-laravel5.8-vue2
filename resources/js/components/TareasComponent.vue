<template>
    <div>
        <h3>Agregar Notas</h3>
        <form @submit.prevent="agregar()">
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
            <button class="btn btn-warning btn-sm">Guardar</button>
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
                <button class="btn btn-danger btn-sm" @click="eliminarNota(item)">
                    Eliminar
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
            const response = await axios.delete(`/notas/${item.id}`);
            if (response) {
                this.notas.splice(index, 1);
            }
        }
    }
};
</script>
