# Reto 4: Archivo ZIP con contraseña en metadatos de imagen (v2.0)

**Tema principal**: Esteganografía en metadatos de imágenes + análisis forense de archivos ZIP.

<br>

## Implementación Paso a Paso:

1.  **Crear el archivo ZIP:**
    
    - Nombre: **Documentos_Importantes.zip**
        
    - Contenido:
        
        - Declaracion_Impuestos_2024.txt (archivo señuelo con texto genérico).
            
        - Foto_Reunion.jpg (imagen que contendrá la clave).
            
2.  **Preparar la imagen (foto_reunion.jpg):**
    
    - Crea una imagen JPEG de aspecto normal (una foto de una reunión, por ejemplo).
        
    - Ocultar la contraseña del ZIP en el campo "UserComment" de los metadatos EXIF utilizando **exiftool**:
        
        ```bash
        exiftool -UserComment="ClaveDesbloqueo: AES_Key_2025" Foto_Reunion.jpg`
        ```
    
3.  **Cifrar el archivo ZIP:**
    
    - Usar 7-Zip o cualquier herramienta similar para crear un archivo ZIP cifrado con la contraseña oculta en la imagen (**AES_Key_2025**).
        
    - Asegurarse de que el nombre del archivo ZIP no revele información útil.
        
4.  **Pistas (opcional):**
    
    - Incluir una pista sutil en el archivo de texto (*Declaracion_Impuestos_2024.txt*): *"La clave para entender los números está en una foto."*
5.  **Archivo final:**
    
    - Documentos_Importantes.zip
        
    - Foto_Reunion.jpg (dentro del ZIP)
        
    - Declaracion_Impuestos_2024.txt (dentro del ZIP)
        
<br>

## Cómo lo resolverían los jugadores:

1.  **Análisis inicial:**
    
    - Descargar el archivo Documentos_Importantes.zip.
        
    - Intentar descomprimir el ZIP y descubrir que está protegido con contraseña.
        
    - Leer el archivo Declaracion_Impuestos_2024.txt y encontrar la pista sobre la foto.
        
2.  **Extracción de metadatos EXIF:**
    
    - Extraer **Foto_Reunion.jpg** del archivo ZIP.
        
    - Usar *exiftool* para leer los metadatos EXIF de la imagen:
        
        ```bash
        exiftool Foto_Reunion.jpg | grep UserComment`
        ```
    
    - Encontrar el campo UserComment que contiene la contraseña: ClaveDesbloqueo: *AES_Key_2025*.
3.  **Descompresión del zip:**
    
    - Usar la contraseña extraída (AES_Key_2025) para descomprimir el archivo ZIP y acceder a su contenido.
<br>

## Variantes para hacerlo más difícil:

- **Cifrado en capas**: La contraseña en el UserComment está cifrada con Base64 o XOR, requiriendo un paso adicional de decodificación.
    
- **Contraseña parcial**: La imagen contiene solo parte de la contraseña, y la otra parte está oculta en el nombre del archivo ZIP o en otro lugar.
    
- **Esteganografía visual**: Además de la contraseña en los metadatos, la imagen contiene la contraseña visualmente oculta usando técnicas de esteganografía.
    
<br>

## Herramientas clave:

- **Análisis de metadatos:** ExifTool.
    
- **Descompresión de ZIP:** 7-Zip, unzip.
    
- **Esteganografía (opcional):** StegSolve, herramientas de esteganografía online.