# Carta - Menú
https://carta-menu.netlify.app

Una aplicación para crear una lista de platos en diferentes tableros, contiene las siguientes funcionalidades:
 
- Creación de un nuevo tablero de platos. Se valida para que no se repita.
- Creación de nuevos platos en el tablero mediante un input y un limitador de 8 caracteres. Se valida para que no se repita.
- Edición del nombre del tablero. Se valida si el nombre está repetido al guardar los tableros
- Borrado del tablero.
- Borrado del plato en un tablero.
- Pinchar y arrastrar platos dentro de un tablero o entre tableros. Si el destino ya tiene el elemento se cancela el drop.

## Configurar el proyecto

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```