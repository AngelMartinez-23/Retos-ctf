# Reto 5: La herramienta olvidada

<br>
 
**Tema principal:** Enumeración de directorios web y uso de herramientas de descubrimiento

## Implementación del reto:

1. **Configurar un servidor web local** con la siguiente estructura:
   - Página principal en `http://localhost/`
   - Directorio oculto `/archivos/`
   - Archivo `flag.txt` dentro de `/archivos/`

2. **Crear el archivo `index.html`** con una pista oculta:

   ```html
      Reto 5

      Bienvenido al Reto 5
      Encuentra la flag oculta
   ```

3. **Crear el archivo `flag.txt`** en `/archivos/`:
   ```
   flag{gobuster_is_the_key}
   ```

## Mecánica de resolución para los jugadores:

1. **Analizar la página principal**:
   - Acceder a `http://localhost/`
   - Inspeccionar el código fuente para encontrar la pista sobre Gobuster

2. **Utilizar Gobuster para escanear directorios**:
   ```bash
   gobuster dir -u http://localhost -w /usr/share/wordlists/dirb/common.txt
   ```

3. **Identificar el directorio `/archivos/`** en los resultados del escaneo

4. **Acceder a `http://localhost/archivos/flag.txt`** para obtener la flag

## Claves pedagógicas:

- ✅ **Uso de herramientas de enumeración web**: Práctica con Gobuster
- ✅ **Análisis de código fuente**: Búsqueda de pistas ocultas
- ✅ **Reconocimiento de estructuras web**: Identificación de directorios ocultos

## Variantes para incrementar la dificultad:

1. Ocultar la pista en un archivo JavaScript o CSS
2. Requerir autenticación básica para acceder al directorio `/archivos/`
3. Implementar una redirección en `/archivos/` a otro subdirectorio oculto