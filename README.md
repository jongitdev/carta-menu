# Carta - Menú
https://carta-menu.netlify.app  
Una aplicación para crear una lista de platos en diferentes tableros, contiene las siguientes funcionalidades:

- Creación de un nuevo tablero de platos.
- Creación de nuevos platos en el tablero mediante un input y un limitador de 8 caracteres.
- Edición del nombre del tablero. 
- Borrado del tablero.
- Borrado del plato en un tablero.
- Pinchar y arrastrar platos dentro de un tablero o entre tableros. 

## Mejoras:
**Validaciones** para evitar repeticiones de:

- Nombres de tableros al crear uno nuevo.
- Al guardar los paneles si se ha editado el nombre de alguno, se valida que tampoco se repita.
- Nombres de platos en un tablero.
- Al pinchar y arrastrar un plato a un tablero se valida que no se repita en tablero de destino.

## Configurar el proyecto
```sh
1 git clone https://github.com/jongitdev/carta-menu.git
2 npm install
```
Paso 1: Clonar el proyecto de Github  
Paso 2: Instalar el proyecto con sus dependecians usando node  
*Si no tienes Node, puede descrgarlo de: https://nodejs.org/es*

### Compilación para desarrollo

```sh
npm run dev
```

### Compilación para despliegue en producción

```sh
npm run build
```