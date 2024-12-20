<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Listado de Usuarios</h5>

        <div class="card">
            <div class="card-body">
                 
                <div class="mb-4">
                    <input 
                        type="text" 
                        class="form-control" 
                        placeholder="Buscar por Nombre o Correo" 
                        v-model="filterText">
                </div>

                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                        <tr>
                            <th colspan="7">Añadir Usuario</th>
                        </tr>
                        <tr>
                            <th>
                                <input type="text" class="form-control" placeholder="ID" v-model="newUser.id">
                            </th>
                            <th>
                                <input type="text" class="form-control" placeholder="Nombre" v-model="newUser.name">
                            </th>
                            <th>
                                <input type="email" class="form-control" placeholder="Correo" v-model="newUser.email">
                            </th>
                            <th>
                                <input type="text" class="form-control" placeholder="Dirección" v-model="newUser.address">
                            </th>
                            <th>
                                <input type="text" class="form-control" placeholder="Teléfono" v-model="newUser.phone">
                            </th>
                            <th>
                                <input type="text" class="form-control" placeholder="País" v-model="newUser.country">
                            </th>
                            <th>
                                <input type="text" class="form-control" placeholder="Ciudad" v-model="newUser.city">
                            </th>
                            <th>
                                <button type="submit" class="btn btn-primary">Agregar</button>
                            </th>
                        </tr>
                        <tr>
                            <th>#</th>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Correo</th>
                            <th>Dirección</th>
                            <th>Teléfono</th>
                            <th>País</th>
                            <th>Ciudad</th>
                            <th>Acciones</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr 
                            v-for="(user, index) in filteredUsers" 
                            :key="index">
                            <td>{{ index + 1 }}</td>
                            <td>{{ user.id }}</td>
                            <td>{{ user.name }}</td>
                            <td>{{ user.email }}</td>
                            <td>{{ user.address }}</td>
                            <td>{{ user.phone }}</td>
                            <td>{{ user.country }}</td>
                            <td>{{ user.city }}</td>
                            <td>
                                <button type="button" class="btn btn-info btn-sm" @click="edit(index)">
                                    <i class="bi bi-pen"></i>
                                </button>
                                <button type="button" class="btn btn-danger btn-sm" @click="remove(index)">
                                    <i class="bi bi-trash3"></i>
                                </button>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

 
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar Usuario</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            <div class="mb-3" v-for="(value, key) in itemSelected" :key="key">
                                <label :for="key" class="form-label">{{ key }}</label>
                                <input :id="key" type="text" class="form-control" v-model="itemSelected[key]">
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newUser: {
                id: '',
                name: '',
                email: '',
                address: '',
                phone: '',
                country: '',
                city: ''
            },
            usersArray: [
                {
                    id: '1',
                    name: 'Alice Jonhson',
                    email: 'alice.jonhson@example.com',
                    address: 'Calle 123',
                    phone: '789456123',
                    country: 'Bolivia',
                    city: 'La Paz'
                },
                {
                    id: '2',
                    name: 'Ana López',
                    email: 'ana.lopez@example.com',
                    address: 'Avenida Siempre Viva',
                    phone: '123456789',
                    country: 'Bolivia',
                    city: 'Santa Cruz'
                },
                {
                id: 3,
                name: "Alice Johnson",
                email: "alice.johnson@example.com",
                address: "123 Maple Street",
                phone: "123-456-7890",
                country: "USA",
                city: "New York",
            },
            {
                id: 4,
                name: "Bob Smith Mash",
                email: "bob.smith@example.com",
                address: "456 Oak Avenue",
                phone: "987-654-3210",
                country: "Canada",
                city: "Toronto",
            },
            ],
            filterText: '', 
            itemSelected: null,
            indexSelected: null
        };
    },
    computed: {
        
        filteredUsers() {
            const text = this.filterText.toLowerCase();
            return this.usersArray.filter(user =>
                user.name.toLowerCase().includes(text) || 
                user.email.toLowerCase().includes(text)
            );
        }
    },
    methods: {
       
        save() {
            if (Object.values(this.newUser).every(value => value.trim())) {
                this.usersArray.push({...this.newUser});
                Object.keys(this.newUser).forEach(key => this.newUser[key] = '');
            } else {
                alert("Completa todos los campos.");
            }
        },
      
        remove(index) {
            if (confirm("¿Seguro de eliminar este usuario?")) {
                this.usersArray.splice(index, 1);
            }
        },
        
        edit(index) {
            this.itemSelected = {...this.usersArray[index]};
            this.indexSelected = index;
            const editModal = new bootstrap.Modal(document.getElementById('editModal'));
            editModal.show();
        },
    
        saveEdit() {
            this.usersArray[this.indexSelected] = {...this.itemSelected};
            const modalElement = document.getElementById('editModal');
            const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
            modalInstance.hide();
            this.itemSelected = null;
            this.indexSelected = null;
        }
    }
};
</script>
