# Reto 2: Binario con Flag en Sección .data Cifrada (v2.0)

**Tema principal**: Ingeniería inversa de binarios + manipulación de secciones + criptografía XOR simple.

<br>

## Implementación paso a paso:

1.  **Crear el programa en C:**

    ```c
    #include <stdio.h>#include <string.h>// Marcar la sección .data__attribute__((section(".mydata"))) char flag_ciphered[] = {    0x4B, 0x1F, 0x09, 0x1E, 0x1D, 0x0D, 0x0A, 0x1B, 0x01, 0x5A,    0x57, 0x4C, 0x5D, 0x59, 0x5B, 0x18, 0x0E, 0x1D, 0x0C, 0x10,        0x06, 0x07, 0x0C, 0x04};int main() {    // Simplemente mostrar un mensaje para distraer    printf("Ejecuta 'objdump -s <nombre_del_ejecutable>' para inspeccionar las secciones.\n");        return 0;}`
    ```

- **Explicación**:
    
    - *_*attribute*\_((section(".mydata")))* coloca **flag_ciphered** en una sección personalizada llamada **.mydata**. Esto la separa de la sección *.data* estándar.
        
    - La flag está cifrada con XOR.
        

2.  **Compilación:**
    
    - Compilar el código con GCC:
        
        ```bash
        gcc -o crackme crackme.c
        ```
        
3.  **Añadir pistas (opcional):**
    
    - Pistas dentro del código, pero no directamente obvias:
        
        ```c
        printf("La respuesta está en una sección no tan estándar.\n");`
        ```
        
4.  **Archivo final:**
    

- **crackme** (el ejecutable compilado).

<br>

## Cómo lo resolverían los jugadores:

1.  **Análisis inicial:**

- Ejecutar el programa:
    
    ```bash
    ./crackme
    ```

    - El programa imprime un mensaje de ayuda.

2.  **Identificación de las secciones:**

- Usar *objdump* para listar las secciones:
    
    ```bash
    objdump -s crackme
    ```
    
    - Buscar la sección **.mydata**.

3.  **Extracción de los datos cifrados:**
    
    - Conocer el offset y tamaño de la sección **.mydata** de la salida de objdump, extraer los bytes relevantes.
4.  **Descifrado XOR:**
    
    - Crear un script en python para realizar el XOR:
    
        ```python
        ciphered_data = [0x4B, 0x1F, 0x09, 0x1E, 0x1D, 0x0D, 0x0A, 0x1B, 0x01, 0x5A,                 0x57, 0x4C, 0x5D, 0x59, 0x5B, 0x18, 0x0E, 0x1D, 0x0C, 0x10,                                  0x06, 0x07, 0x0C, 0x04]key = 0x42  # Clave XOR (la más común en CTFs, ¡ojo!)decrypted_flag = ''.join(chr(byte ^ key) for byte in ciphered_data)print(decrypted_flag)  # -> "FLAG{SECCIONES_CUSTOM}"`
        ```
    
<br>


## Variantes para hacerlo más difícil:

- **Múltiples secciones**: Distribuir la flag en múltiples secciones personalizadas.
    
- **Clave dinámica**: Calcular la clave XOR a partir de variables del sistema (hora, nombre de usuario, etc.).
    
- **Código auto-modificable**: Hacer que el programa modifique el código XOR en memoria antes de descifrar la flag.  

<br/>
    

## Herramientas Clave:

- **Análisis estático:** objdump, readelf, radare2, Ghidra.
    
- **Análisis dinámico:** gdb, ltrace, strace.
    
- **Scripting:** Python, Bash.