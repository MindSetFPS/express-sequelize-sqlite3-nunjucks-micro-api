<template>
    <div>
        <div>
            <div class="bg-white rounded-md mt-2 px-2 py-1 text-xs"  
                v-if="customer && customer.toLowerCase().includes(customerFilter.toLowerCase()) && locationFilter ==  '' || locationFilter == location " 
            >
            <div class="" >
                <h2 class="text-xl" >{{ customer }}</h2>
                <h3 class="text-md" >{{ createdAt }} </h3>
                <h3 class="text-xs" > {{ location }} </h3>
            </div>
                <div style="flex  justify-items: center; align-items: center; " class="flex justify-between" >
                    <div> 
                        <div >    
                            <button
                                @click="handleClick('changeDelivered')"
                                class="bg-gray-100 hover:bg-gray-200 p-1 rounded-sm mb-1 w-full "
                            >
                                <p :class="delivered ? 'text-green-600' : 'text-red-500'" >
                                    {{ delivered ? 'Entregado' : 'No entregado' }}
                                </p>
                            </button>
                            <br />
                            <button
                                @click="handleClick('changePaid')"
                                class="bg-gray-100 hover:bg-gray-200 p-1 rounded-sm w-full "
                            >
                                <p :class="paid ? 'text-green-700' : 'text-red-500'" >
                                    {{ paid ? 'Pagado  ' : 'No pagado' }}
                                </p>
                            </button>
                        </div>
                        <div class="flex mt-1 " >
                            <button class="bg-gray-100 p-1 hover:bg-gray-200 w-full mr-1" @click="updatePedido" >
                                Actualizar
                            </button>
                            <button class="p-1 bg-gray-100 hover:bg-gray-200 w-full " >
                                <router-link :to="'/details/' + pedidoId" > 
                                    Detalles
                                </router-link>
                            </button>
                        </div>
                    </div>
                    <div>
                        <h3>{{ comida0 }}</h3>
                    </div>
                    <div>
                        <h3>{{ comida1 }}</h3>
                    </div>
                    <h3> $ {{ ( comida0 + comida1 ) * 45 }}</h3>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'PedidoItem',
    props: ["pedidoId", "customerFilter", "locationFilter", "location", "customer", "paid", "delivered", "createdAt", "comida0", "comida1"],
    data(){
        return{
            api: process.env.VUE_APP_API
        }
    },
    watch: {
        paid(val, oldVal){
            console.log(val, oldVal)
            this.updatePedido()
        },
        delivered(val, oldVal){
            console.log(val, oldVal)
            this.updatePedido()
        }
    },
    methods: {
        //Note: Local storage management was done here, it will have to me rewriten.
        async updatePedido(){
            const url = '/pedidos/api/update/'
            const update = await fetch( this.api + url + this.pedidoId , {
                headers: {'Content-Type': 'application/json'},
                method: 'POST',
                body: JSON.stringify({delivered: this.delivered, paid: this.paid})
            }).then(res => res.json())
            .catch(
                err => console.error(err)
            )
            console.log(update)
        },
        handleClick(change){
            this.$emit(change, this.pedidoId)
        }
    },
}
</script>