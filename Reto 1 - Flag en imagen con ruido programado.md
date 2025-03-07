# Reto 1 - Flag en imagen con ruido programado

**Tema principal**: Esteganografía avanzada en imágenes + generación pseudoaleatoria + análisis estadístico.

<br>

## Implementación paso a paso:

1.  **Imagen base**:
    
    - Elige una imagen PNG o JPG de baja complejidad visual (ej., un degradado suave, una textura sutil).
        
    - Nombre: **Background.png**
        
2.  **Generación de ruido específico**:
    
    - En lugar de ruido aleatorio general, genera ruido basado en una función pseudoaleatoria controlada por una semilla:
    
        ```python
        from PIL import Imageimport numpy as npdef generate_noise_pattern(width, height, seed=12345):    np.random.seed(seed)        return np.random.randint(-10, 11, size=(height, width, 3), dtype=np.int8)  # Ajuste para coloreswidth, height = 512, 512  # Tamaño de la imagennoise_pattern = generate_noise_pattern(width, height)base_image = Image.new("RGB", (width, height), color=(128, 128, 128))  # Fondo gris neutronoisy_image = np.array(base_image, dtype=np.int16) + noise_patternnoisy_image = np.clip(noisy_image, 0, 255).astype(np.uint8)  # Asegurar valores válidosImage.fromarray(noisy_image).save("noisy_background.png")`
        ```
    
    - **Explicación**:
        
        - La función **generate_noise_pattern** crea un patrón de ruido repetible basado en la semilla.
            
        - El ruido se añade a una imagen base neutra (gris) para facilitar la manipulación.
            
3.  **Incrustación de la flag**:
    
    - Ocultar la flag en los bits menos significativos (LSB) **solo donde el ruido es alto**:
    
        ```python
        def embed_flag_in_noisy_pixels(image_path, flag, seed=12345, noise_threshold=5):    image = Image.open(image_path).convert("RGB")    pixels = np.array(image)        np.random.seed(seed)    flag_binary = ''.join(format(ord(char), '08b') for char in flag)  # Convertir flag a binario    width, height = image.size    total_pixels = width * height        flag_index = 0    for y in range(height):        for x in range(width):            # Calcular diferencia de ruido para cada color            noise_diff = np.sum(np.abs(pixels[y, x] - 128))            if noise_diff > noise_threshold: # Usar píxeles con "suficiente" ruido                if flag_index < len(flag_binary):                    bit = int(flag_binary[flag_index])                                        # Modificar el LSB del componente rojo para ocultar el bit                    pixels[y, x, 0] = (pixels[y, x, 0] & ~1) | bit                    flag_index += 1                else:                    break        if flag_index >= len(flag_binary):                        break  # Asegurar que no se exceda la flag        Image.fromarray(pixels).save("flag_embedded.png")`
        ```
    
    - **Explicación**:
        
        - La función **embed_flag_in_noisy_pixels** recorre los píxeles.
            
        - Solo modifica el LSB de los píxeles donde la "diferencia de ruido" (respecto al valor base) es alta. Esto hace que la esteganografía sea más difícil de detectar visualmente.
            
4.  **Metadatos**:
    
    - Insertar metadatos EXIF con información relevante (pero confusa):
    
        ```bash
        exiftool -Comment="Ruido generado con semilla = 12345, umbral ruido > 5" flag_embedded.png`
        ```
    
5.  **Archivo final**: ***flag_embedded.png***
    
<br>

## Cómo lo resolverían los jugadores:

1.  **Análisis inicial:**
    
    - Inspeccionar la imagen visualmente (parece solo ruido).
        
    - Usar **exiftool** para ver los metadatos. La semilla y el umbral de ruido son pistas.
        
2.  **Análisis estadístico:**
    
    - Intentar detectar patrones LSB con **zsteg**: *zsteg -a flag_embedded.png* (puede no funcionar debido al ruido).
3.  **Extracción de la flag:**
    
    ```python
    def extract_flag_from_noisy_pixels(image_path, flag_length, seed=12345, noise_threshold=5):    image = Image.open(image_path).convert("RGB")    pixels = np.array(image)    np.random.seed(seed)    flag_binary = ''    width, height = image.size        found_bits = 0    for y in range(height):        for x in range(width):            noise_diff = np.sum(np.abs(pixels[y, x] - 128))            if noise_diff > noise_threshold:                flag_binary += str(pixels[y, x, 0] & 1) # Extraer LSB del rojo                found_bits += 1                if found_bits >= flag_length * 8:                    break # Suficientes bits extraídos        if found_bits >= flag_length * 8:                        break    # Convertir binario a texto    flag = ''.join([chr(int(flag_binary[i*8: (i+1)*8], 2)) for i in range(flag_length)])        print("Bandera extraída:", flag)flag_length = 27 # Longitud esperada de la flag (ajustar)extract_flag_from_noisy_pixels("flag_embedded.png", flag_length)`
    ```
    
<br>

## Variantes para hacerlo más difícil:

- **Umbral dinámico**: El umbral de ruido depende de la posición del píxel.
    
- **Múltiples canales**: Ocultar bits de la flag en R, G y B de forma alternada.
    
- **Cifrado XOR**: Cifrar la flag antes de insertarla con una clave derivada de la semilla.
    

## Herramientas clave:

- **Análisis estadístico:** StegExpose, RS Analysis.
    
- **Manipulación de imágenes:** PIL/Pillow, OpenCV.
    
- **Metadatos:** ExifTool.