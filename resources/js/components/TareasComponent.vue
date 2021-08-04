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
    methods: {
        agregar() {
            const me = this;
            const nuevaNota = me.nota;
            const { nombre, descripcion } = nuevaNota;
            if (nombre.trim() === "" || descripcion.trim() === "") {
                alert(me.val.empty);
                return;
            }
            me.nota = { nombre: "", descripcion: "" };

            console.log(nuevaNota);
            axios.post("/notas", nuevaNota);
        }
    }
};
</script>
