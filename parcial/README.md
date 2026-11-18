# Julian Guajardo 18/11/2026

# Stock del almacén

Aplicación web estática para controlar el stock de un almacén, empaquetada en una imagen Docker liviana con Nginx.

## Contenido del proyecto

```
.
├── index.html   # Aplicación completa (HTML + CSS + JavaScript)
├── Dockerfile   # Imagen basada en nginx:alpine
└── README.md
```

## Qué hace la aplicación

- **Bienvenida y resumen:** muestra el total de productos y unidades, y cuántos necesitan reposición. Se actualiza en vivo.
- **Alta de productos:** formulario con nombre, cantidad inicial y stock mínimo.
- **Listado interactivo:** botones **+** y **−** para ajustar la cantidad y botón **Eliminar** para quitar un producto.
- **Estado por producto**, según su cantidad y su mínimo:

  | Estado       | Condición                      |
  |--------------|--------------------------------|
  | Disponible   | cantidad mayor al mínimo       |
  | Reponer      | cantidad menor o igual al mínimo |
  | Agotado      | cantidad igual a 0             |

- **Persistencia:** los datos se guardan en el `localStorage` del navegador, así que sobreviven a recargas y reinicios del contenedor.
- **Datos de ejemplo:** la primera vez se cargan cuatro productos para ver la app funcionando.
- **Extras de interfaz:** diseño adaptable a celular, modo oscuro automático según el sistema y foco visible para navegar con teclado.

No usa librerías ni dependencias externas.

## Cómo ejecutarla

### Sin Docker

Abrí `index.html` directamente en el navegador.

### Con Docker

Desde la carpeta que contiene `Dockerfile` e `index.html`:

```bash
# Construir la imagen
docker build -t stock-app .

# Ejecutar el contenedor (puerto 8080 del host -> 80 del contenedor)
docker run -d -p 8080:80 --name stock-app stock-app
```

Luego entrá a http://localhost:8080.

Para detener y eliminar el contenedor:

```bash
docker stop stock-app
docker rm stock-app
```

## El Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

- `nginx:alpine` es una imagen base muy liviana.
- `COPY` deja la página en el directorio público de Nginx.
- No hace falta definir `CMD`: la imagen ya inicia Nginx en primer plano.

## Personalización

- **Productos iniciales:** editá el arreglo `seed` dentro del `<script>` de `index.html`.
- **Puerto:** cambiá el primer número de `-p 8080:80` por el que prefieras.
- **Restablecer los datos:** borrá los datos del sitio en el navegador (o ejecutá `localStorage.removeItem('stock-almacen-v1')` en la consola).

## Limitaciones

- Los datos viven en el navegador de cada persona: no se comparten entre dispositivos ni usuarios.
- Para compartir el stock entre varias personas haría falta agregar un backend y una base de datos.
