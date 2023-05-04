<script setup>
    import {reactive, watch} from 'vue';
    import InputView from './InputView.vue';
        /* import the fontawesome core */
    import { library } from '@fortawesome/fontawesome-svg-core';
    /* import font awesome icon component */
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
    /* import specific icons */
    import { faPenToSquare, faTrashCan } from '@fortawesome/free-solid-svg-icons';
    /* add icons to the library */
    library.add(faPenToSquare, faTrashCan)
    import TableroForm from './TableroForm.vue';
    let soltado = false;
    let  tableros = reactive([
        { id:crypto.randomUUID(), nombre: 'Desayuno',
            editar : false,
            items : [{ id:crypto.randomUUID(), elemento : 'pan'},
            { id:crypto.randomUUID(), elemento : 'leche'},
            { id:crypto.randomUUID(), elemento : 'cafe'}
            ]
        },
        { id:crypto.randomUUID(), nombre: 'Primer Plato',
            editar : false,
            items : [{ id:crypto.randomUUID(), elemento : 'sopa'},
            { id:crypto.randomUUID(), elemento : 'ensalada'},
            { id:crypto.randomUUID(), elemento : 'arroz'}
            ]
        }

    ]);

    
    if (localStorage.getItem('tableros') !== null) {
           tableros = reactive(JSON.parse(localStorage.getItem('tableros')));
    }

    
    
    function   handleNewItem(texto,tablero) {
        console.log(tablero);
        
    if (texto.value !== "" ) {
    
    /// Validamos que el nuevo item no esté repetido en el tablero
    if(tablero.items.findIndex(item => texto.value.toLowerCase() === item.elemento.toLowerCase()) == -1){
                tablero.items.push(
                  {id:crypto.randomUUID(), elemento : texto.value}  
               
                );
              } else {
                
                alert(`El plato ${texto.value} ya existe en ${tablero.nombre}`);
              }
    
                
                texto.value = "";
                
                return;
            }
            if (texto.length >= MAX)
            {
                console.log('te pasaste');
                alert('ya no puedes meter más caracteres');
                return;
            }
    }
    function deleteTag(text,tablero) {
       /*  lista = lista.filter((item) => item !== text); */
       console.log(tablero.items);
       console.log(text);
       // const index = tablero.items.id.indexOf(text);
        const index = tablero.items.findIndex((item) => item.id === text);
        console.log(index);
        if (index !== -1) {
            console.log('borrar')
            tablero.items.splice(index, 1);
        }
       

    }    
    
    function onDrop(evt,board,productoId){
        console.log('ondrop')
        const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData("item")); // 
        console.log({ boardId, itemId });
        const tableroOrigen  = tableros.find((tablero) => tablero.id === boardId);
        const productoOrigen  = tableroOrigen.items.find((item) => item.id === itemId); 
        console.log('nombre ' + tableroOrigen.nombre )
        console.log('producto '+ productoOrigen.elemento )      
        tableroOrigen.items = tableroOrigen.items.filter((i) => i.id !== productoOrigen.id);
        //console.log("productoOrigen");
        console.log(productoOrigen.elemento);
        console.log(board.items);
        

        /// Soltar en la posicion arrastrada en la lista de platos
        console.log('productoId' + productoId)
        const indice = board.items.findIndex(p => p.id === productoId);
        console.log(indice)
        if (productoId == 0){ // Si no hay elementos hacemos push
             board.items.push(productoOrigen); // Se carga al final
        } else{

            if(board.items.findIndex(item => productoOrigen.elemento.toLowerCase() === item.elemento.toLowerCase()) == -1){
                let parte1 = board.items.filter((i,index)=> index <=  indice);
                console.log(parte1);
                parte1.push(productoOrigen);
                console.log(parte1);
                const parte2 = board.items.filter((i,index)=> index >  indice);
                console.log(parte2);
                board.items = parte1.concat(parte2);
              } else {
                
                const tableroOrigen = tableros.filter(tablero => tablero.id === boardId);
                tableroOrigen[0].items.push(productoOrigen);
                
                alert(`Ya existe el elemento "${productoOrigen.elemento}" en este tablero`);
                
              }


            
        }

    }

    function startDrag(evt,boardId,itemId) {
        console.log('start drag');
        console.log(boardId, itemId);
        evt.dataTransfer.dropEffect = "move";
        evt.dataTransfer.effectAllowed = "move";
        console.log(JSON.stringify({ boardId, itemId }));
        evt.dataTransfer.setData("item", JSON.stringify({ boardId, itemId }));
        
    }

    function nuevaLista()
    {
        const textoLista = prompt('Ingrese el texto de la lista');
        if (textoLista) {
            const tablero = {
                id: crypto.randomUUID(),
                nombre : textoLista,
                items : []
            } 

            /// Validamos que la nueva lista no esté repetida
    if(tableros.findIndex(tablero => textoLista.toLowerCase() === tablero.nombre.toLowerCase()) == -1){
                    tableros.push(tablero);
              } else {
                
                alert(`El tablero ${textoLista} ya existe`);
              }

            
        }  
    }

    function guardarTablero(){
        //Cerramos la edicion de todos los tableros antes de guardar
        tableros.forEach(tablero => {
            
                tablero.editar = false;
           
        });

        var valueArr = tableros.map(function(item){ return item.nombre });
        var isDuplicate = valueArr.some(function(item, idx){ 
            return valueArr.indexOf(item) != idx 
        });

        if (isDuplicate){
            alert(`Hay tablero repetidos, corrija el nombre de los que se llamen igual`)
        } else {
            localStorage.setItem('tableros', JSON.stringify(tableros));
            alert(`Se han guardado los tableros`)
        }
    }

    function borrarTablero(tablero){
        if (confirm('¿ Estas seguro de borrar el tablero "' + tablero.nombre + '"?')) {  
            let i = tableros.map(item => item).indexOf(tablero) // find index of your object
            tableros.splice(i, 1) // remove it from array
        }
    }

   
    
    
            
         
  

</script>

<template>
<div>
    <button @click="nuevaLista">Nueva lista</button>
    <button @click="guardarTablero">Guardar Tableros</button>
   
    <div class="lista" v-for="tablero in tableros" >
        
      






        <h1>{{ tablero.nombre  }} 
            
            <font-awesome-icon icon="fa-solid fa-pen-to-square" size="2xs" id="formId" style="color: #288649;" @click="tablero.editar = !tablero.editar" />
            <font-awesome-icon icon="fa-solid fa-trash-can" size="2xs" style="color: #288649;" @click="borrarTablero(tablero)"/>

        </h1>
        <TableroForm :tablero="tablero" v-show="tablero.editar" />
        <InputView @onNewItem="(item)=>handleNewItem(item,tablero)" />
          
      
        <div class="tags"
            @dragover.prevent
            @dragenter.prevent 
        
        
        >
        <div v-if="tablero.items.length==0" draggable="true"  @drop="onDrop($event,tablero,0)" >No hay elementos</div>
            <div class="tag" v-for="producto in tablero.items" 
            draggable="true"
            @dragstart="startDrag($event, tablero.id, producto.id)"
            @drop="onDrop($event,tablero,producto.id)"
            >

            {{ producto.elemento }} <button @click="() => deleteTag(producto.id,tablero)">X</button>

            </div>
        </div>
    </div>

    </div>

</template>

<style scoped>
.lista {
    margin :5px;
    border : 1px solid black;
    padding: 2px 5px;
    background-color: aquamarine;

}

.tag {
    margin : 1px;
    border-radius: 3px;
    border : 1px solid blue;
    background : whitesmoke;
}


</style>