# Reto 3 - QR dinámico con frames encriptados (v2.0)

**Tema principal**: Criptografía AES + manipulación de GIFs animados + reconstrucción de QR.

<br>

## Implementación paso a paso:

1.  **Generar la flag y el código QR**:
    
    - La flag será: **FLAG{QR_D1N4M1C0_C0N_AES}**
        
    - Generar un código QR con esta flag (tamaño 256x256).
        
2.  **Dividir el código QR en frames**:
    
    - Dividir el QR en 8 fragmentos horizontales.
        
    - Guardar cada fragmento como un frame PNG separado:
        
        ```python
        from PIL import Imageimport os# Dividir el QR en N fragmentos horizontalesN = 8qr_image = Image.open("qr.png")width, height = qr_image.sizefragment_height = height // Nfor i in range(N):    # Calcular las coordenadas del fragmento    top = i * fragment_height    bottom = (i + 1) * fragment_height    # Recortar el fragmento        fragment = qr_image.crop((0, top, width, bottom))    # Guardar el fragmento como imagen PNG        fragment.save(f"qr_fragment_{i}.png")`
        ```
    
3.  **Cifrar cada frame con AES-ECB**:
    
    - Usar AES-ECB para cifrar cada frame. La clave AES se derivará de una cadena conocida (ej. *"CTF2025_AES_KEY"*):
    
        ```python
        from Crypto.Cipher import AESfrom Crypto.Util.Padding import padimport osKEY = b"CTF2025_AES_KEY"  # Debe ser de 16 bytesdef aes_encrypt_image(input_image_path, key):    # Leer la imagen como bytes    with open(input_image_path, 'rb') as f:        image_data = f.read()        # Crear objeto AES en modo ECB    cipher = AES.new(key, AES.MODE_ECB)        # Asegurar que la longitud de los datos sea múltiplo de 16 bytes    padded_data = pad(image_data, AES.block_size)        # Cifrar los datos    ciphered_data = cipher.encrypt(padded_data)        # Escribir los datos cifrados en un nuevo archivo    output_image_path = os.path.splitext(input_image_path)[0] + "_enc.png"    with open(output_image_path, 'wb') as f:        f.write(ciphered_data)        return output_image_path# Encriptar cada fragmentoencrypted_fragments = []for i in range(N):    input_path = f"qr_fragment_{i}.png"    output_path = aes_encrypt_image(input_path, KEY)        encrypted_fragments.append(output_path)`
        ```
    
4.  **Crear el GIF animado**:
    
    - Crear un GIF animado con los frames cifrados.
    
        ```python
        from PIL import Imageimport imageioimport osdef create_gif(image_paths, output_path, duration=200):    # Leer todas las imágenes    images = [imageio.imread(image_path) for image_path in image_paths]        # Crear el GIF        imageio.mimsave(output_path, images, duration=duration) # duration en milisegundos# Listar los fragmentos cifradosencrypted_images = [f"qr_fragment_{i}_enc.png" for i in range(N)]# Crear el GIF animadocreate_gif(encrypted_images, "dynamic_qr.gif")`
        ```
    
5.  **Añadir pistas en metadatos**:
    
    - Ocultar la clave parcial en los metadatos del GIF. En este caso se oculta la clave codificada en hexadecimal para que el jugador vea que se ha usado AES pero no sepa la clave:
    
        ```bash
        exiftool -Comment="Cifrado con AES-ECB, pero ¡la clave no es tan simple!" dynamic_qr.gif`
        ```
    
6.  **Archivo final:**
    
    - *dynamic_qr.gif*

## Cómo lo resolverían los jugadores:

1.  **Análisis inicial:**
    
    - Al abrir el GIF, se ve una animación de fragmentos de QR que no forman un QR legible.
        
    - Usar **exiftool** para ver los metadatos (pista sobre AES-ECB).
        
2.  **Extracción de los frames:**
    
    - Extraer los frames individuales del GIF:
        
        ```bash
        # Extraer los frames con convert (ImageMagick)convert dynamic_qr.gif frame_%02d.png`
        ```
        
3.  **Descifrado AES-ECB:**
    
    - Descifrar cada frame con AES-ECB usando la clave "CTF2025_AES_KEY".
    
        ```python
        from Crypto.Cipher import AESfrom Crypto.Util.Padding import unpadKEY = b"CTF2025_AES_KEY"def aes_decrypt_image(input_image_path, key):    with open(input_image_path, 'rb') as f:                encrypted_data = f.read()    # Crear objeto AES en modo ECB        cipher = AES.new(key, AES.MODE_ECB)    # Descifrar los datos        decrypted_data = cipher.decrypt(encrypted_data)    # Eliminar el relleno        unpadded_data = unpad(decrypted_data, AES.block_size)    # Escribir los datos descifrados en un nuevo archivo    output_image_path = os.path.splitext(input_image_path)[0].replace("_enc", "") + "_dec.png"    with open(output_image_path, 'wb') as f:        f.write(unpadded_data)        return output_image_path`
        ```
    
4.  **Reconstrucción del QR:**
    
    - Reconstruir el código QR combinando los frames descifrados en el orden correcto.
5.  **Lectura del QR:**
    
    - Escanear el código QR reconstruido para obtener la flag.

<br>

## Variantes para hacerlo más difícil:

- **Orden aleatorio**: Mezclar el orden de los frames en el GIF (requiere descubrir el orden correcto).
    
- **Clave derivada**: La clave AES es el hash SHA-256 de una cadena oculta en los metadatos del GIF.
    
- **Cifrado asimétrico**: Usar RSA para cifrar los frames (requiere crackear la clave privada).
    
<br>

## Herramientas clave:

- **Criptografía:** PyCryptodome (para AES).
    
- **Manipulación de imágenes:** PIL (Pillow), ImageMagick.
    
- **Análisis de metadatos:** ExifTool.
    
- **Lectura de QR:** zbarimg.