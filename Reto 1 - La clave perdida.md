# Reto 1 - La clave perdida

<br>

**Tema principal:** Análisis forense y cracking de contraseñas

## **Implementación en 3 pasos**:

1. **Crear archivo ZIP protegido** (en Linux/Mac):
```bash
echo "flag{zip_cracked}" > flag.txt
zip -e clave_perdida.zip flag.txt
```
*Al pedir contraseña:* Usar **`p4ssCTF`** (fácil de incluir en diccionarios)

2. **Generar diccionario básico** (`diccionario.txt`):
```
123456
password
p4ssCTF
admin
secret
```

3. **Insertar pista en metadatos**:
```bash
exiftool -Comment="Posible contraseña: 8 caracteres combinando números y letras" clave_perdida.zip
```

### **Mecánica de resolución**:

1. **Analizar metadatos**:
```bash
exiftool clave_perdida.zip
```
*Verán la pista de 8 caracteres alfanuméricos*

2. **Ataque de diccionario** con `fcrackzip`:
```bash
fcrackzip -u -D -p diccionario.txt clave_perdida.zip
```
*Encontrará la contraseña en segundos*

3. **Extraer flag**:
```bash
unzip clave_perdida.zip
cat flag.txt  # Muestra flag{clave_maestra}
```

## **Claves pedagógicas**:
- ✅ **Dificultad ajustada**: Contraseña en diccionario básico
- ✅ **Pista útil pero no reveladora**: Longitud y formato en metadatos
- ✅ **Herramienta sencilla**: `fcrackzip` (preinstalada en Kali Linux)

## **Variante para mayor desafío**:
- Usar contraseña leet modificada: **`p4$5CTF!`**
- Incluir archivos dummy con falsas flags
- Comprimir con cifrado AES-256 (`-P` en zip)

## **Testing esencial**:
```bash
# Verificar que el ataque funciona
fcrackzip -v -u -D -p diccionario.txt clave_perdida.zip
# Debe mostrar: PASSWORD FOUND!!!!: pw == p4ssCTF
```