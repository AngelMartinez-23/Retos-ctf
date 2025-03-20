# Reto 4: La API Misteriosa

<br>

**Tema principal:** Automatización de pruebas de API y fuerza bruta

## Implementación del reto:

1. **Crear el script de Python** (`api_misteriosa.py`):

```python
import requests

url = "https://api.ctf.com/secret"
base_clave = "CTF-"

for i in range(10000):
    clave = f"{base_clave}{i:04d}"
    r = requests.get(url, params={"key": clave})
    if "flag{" in r.text:
        print(f"Clave encontrada: {clave}")
        print(f"Flag: {r.text}")
        break
    if i % 100 == 0:
        print(f"Probando clave: {clave}")
```

2. **Proporcionar un archivo de pista** (`pista.txt`):
```
Pista: La clave sigue el formato CTF-XXXX donde XXXX es un número de 4 cifras.
Considera usar un bucle para probar todas las combinaciones posibles.
```

## Mecánica de resolución para los jugadores:

1. **Analizar el código proporcionado**:
   - Identificar la estructura de la URL y los parámetros necesarios.
   - Notar el formato de la clave mencionado en la pista.

2. **Implementar un script de fuerza bruta**:
   - Crear un bucle que genere todas las combinaciones posibles (0000 a 9999).
   - Utilizar el módulo `requests` para enviar peticiones GET a la API.
   - Verificar la respuesta en busca de la flag.

3. **Ejecutar el script y esperar el resultado**:
   - El script probará todas las combinaciones hasta encontrar la correcta.
   - Una vez encontrada, mostrará la clave y la flag.

## Claves pedagógicas:

- ✅ **Automatización de pruebas de API**: Uso práctico del módulo `requests`.
- ✅ **Técnicas de fuerza bruta**: Implementación de un ataque de diccionario simple.
- ✅ **Manejo de respuestas HTTP**: Análisis del contenido de las respuestas.

## Variantes para incrementar la dificultad:

1. Implementar un límite de intentos o un retraso entre peticiones para evitar bloqueos.
2. Requerir autenticación adicional o manejar tokens de sesión.
3. Ocultar la flag en diferentes formatos de respuesta (JSON, XML, etc.).

## Herramientas útiles:

- Python con el módulo `requests`
- Postman o cURL para pruebas manuales de API
- Un editor de texto o IDE para escribir y ejecutar el script
