# Reto 5: Flag en capa de ajuste de PNG (v2.0)

**Tema principal**: Esteganografía avanzada en imágenes + manipulación de capas en PNG + teoría del color.

<br>

## Implementación paso a paso:

1.  **Crear la imagen base:**
    
    - Elige una imagen en escala de grises (para simplificar la manipulación del color).
        
    - Tamaño: 512x512 píxeles.
        
    - Nombre: **base_image.png**
        
2.  **Preparar la flag:**
    
    - La flag será: *FLAG{C4P4S_S3CR3T4S_PNG}*
        
    - Convertir la flag en una imagen PNG en escala de grises (ej: texto blanco sobre fondo negro).
        
    - Nombre: **flag_image.png**
        
3.  **Crear capas en GIMP:**
    
    - Abrir *base_image.png* en GIMP.
        
    - Añadir *flag_image.png* como una nueva capa.
        
    - Ocultar la *flag_image.png* utilizando una **capa de ajuste "Curvas"**:
        
        - Añadir una nueva capa de ajuste "Curvas" sobre la capa flag_image.png.
            
        - Ajustar la curva para que la flag_image.png sea casi invisible (ejemplo: invertir la curva).
            
        - Establecer el modo de fusión de la capa de ajuste a "Multiplicar" o "Superponer".
            
4.  **Exportar el PNG:**
    
    - Exportar la imagen como PNG, conservando las capas.
        
    - Nombre: *final_image.png*
        
5.  **Añadir pistas:**
    
    - Insertar una pista sutil en los metadatos PNG:
        
        ```bash
        exiftool -Comment="La clave está en el ajuste... y en el color." final_image.png`
        ```
        
<br>

## Cómo lo resolverían los jugadores:

1.  **Análisis inicial:**
    
    - Inspeccionar visualmente *final_image.png* (parece una imagen en escala de grises normal).
        
    - Usar **exiftool** para ver los metadatos (pista sobre ajuste y color).
        
2.  **Análisis de capas:**
    
    - Abrir **final_image.png** en GIMP (o Photoshop).
        
    - Inspeccionar las capas: identificar la capa base y la capa con la flag.
        
    - Notar la capa de ajuste "Curvas".
        
3.  **Manipulación de la capa de ajuste:**
    
    - Modificar la capa de ajuste "Curvas" para revelar la flag_image.png.
        
    - Invertir la curva para hacer visible la flag.
        
    - Ajustar el modo de fusión de la capa de ajuste si es necesario.
        
4.  **Lectura de la flag:**
    

- Una vez que la **flag_image.png** sea visible, leer la flag.

<br>

## Variantes para hacerlo más difícil:

- **Múltiples capas de ajuste**: Usar varias capas de ajuste para hacer la flag aún más difícil de ver.
    
- **Modos de fusión complejos**: Experimentar con diferentes modos de fusión para ocultar la flag de manera más efectiva.
    
- **Color grading avanzado**: Usar capas de ajuste de color para modificar los canales RGB y ocultar la flag en un canal específico.
    
- **Esteganografía adicional**: Ocultar una pista en los bits menos significativos (LSB) de la imagen base.
    
<br>

## Herramientas clave:

- **Editor de imágenes:** GIMP (gratuito), Photoshop (de pago).
    
- **Análisis de metadatos:** ExifTool.
    
- **Esteganografía (opcional):** StegSolve.