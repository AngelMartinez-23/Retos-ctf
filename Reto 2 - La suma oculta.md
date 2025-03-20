# Reto 2: La suma oculta

<br>

**Tema principal:** Manipulación de datos y conversión entre sistemas numéricos

## Implementación del reto:

1. **Crear el script de Python** (`suma_oculta.py`):

```python
a = 25
b = 17
resultado = a + b

print(f"flag{{{hex(resultado)[2:]}}}")
```

2. **Ejecutar el script**:
```bash
python suma_oculta.py
```
Salida: `flag{2a}`

3. **Proporcionar pista en un archivo separado** (`pista.txt`):
```
Pista: Convierte el resultado de hexadecimal a decimal.
Recuerda que en Python, hex() añade '0x' al principio del número hexadecimal.
```

## Mecánica de resolución para los jugadores:

1. **Analizar el script**:
   - Identificar que se suman dos números: 25 y 17.
   - Notar que el resultado se convierte a hexadecimal antes de imprimirse.

2. **Ejecutar el script** para obtener la flag encriptada:
   ```
   flag{2a}
   ```

3. **Convertir el resultado**:
   - Convertir '2a' de hexadecimal a decimal: 2a (hex) = 42 (dec)
   - Verificar: 25 + 17 = 42

4. **Comprobar la solución**:
   - Confirmar que 42 en hexadecimal es efectivamente '2a'

## Claves pedagógicas:

- ✅ **Comprensión de sistemas numéricos**: Hexadecimal vs Decimal
- ✅ **Manipulación básica de strings en Python**: Uso de f-strings y slicing
- ✅ **Pensamiento lógico**: Deducir la operación inversa necesaria

## Variantes para incrementar la dificultad:

1. Usar números más grandes o operaciones más complejas.
2. Aplicar un cifrado adicional al resultado hexadecimal.
3. Ocultar parte del código fuente, dejando solo la salida encriptada.

## Herramientas útiles:

- Python (para ejecutar y analizar el script)
- Calculadora con modo programador (para conversiones hex/dec)
- Sitios web de conversión de bases numéricas (como RapidTables)
