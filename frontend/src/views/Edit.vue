<template>
    <div class="">
        <div v-if="editType == 'pedido'" >
            <pedido-edit-or-create :pedidoId="$route.params.id" />
        </div>

        <div v-if="editType == 'food'" >
            <h2>Esto es un {{ editType }}</h2>
            <food-edit-or-create />
        </div>

        <div v-if="editType == 'calendar'" >
            <h2>This is  {{ editType }}</h2>
        </div>
        
        <div v-if="editType == 'account'" >
            <h2>This is {{ editType }}</h2>
        </div>

    </div>
</template>

<script>
import FoodEditOrCreate from '../components/FoodEditOrCreate.vue'
import PedidoEditOrCreate from '../components/PedidoEditOrCreate.vue'
export default {
    name: 'Edit',
    props: ['editType'],
    components: {
        FoodEditOrCreate,
        PedidoEditOrCreate
    },
    methods: {
        async deletePedido(){
            const res = await fetch( this.api + '/pedidos/api/update/' + this.pedidoId, {
                headers: {'Content-Type': 'application/json'},
                method: 'DELETE',
            }).then(res => res.json()).catch(e => console.error(e))
            
            this.res = res
        }
    }
}
</script>