# Reto 3: Bucle Infinito

<br>

**Tema principal:** Depuración y análisis de código

## Implementación del reto:

1. **Crear el script de Python** (`bucle_infinito.py`):

```python
x = 0
while True:
    x += 1
    if x == 0x539:  # 1337 en hexadecimal
        print(f"flag{{{x * 2}}}")
    # El bucle nunca termina
```

2. **Proporcionar el script a los participantes** sin ejecutarlo.

3. **Crear un archivo de pista** (`pista.txt`):
```
Pista: Modifica el código o usa un depurador para encontrar la flag sin ejecutar el bucle infinito.
Recuerda que los números hexadecimales en Python comienzan con '0x'.
```

## Mecánica de resolución para los jugadores:

1. **Analizar el código**:
   - Identificar el bucle infinito (`while True`).
   - Notar la condición `if x == 0x539`.
   - Observar que la flag se genera como `x * 2`.

2. **Estrategias de resolución**:

   a) **Modificación del código**:
      - Cambiar `while True` por `if x == 0x539`.
      - Ejecutar el script modificado.

   b) **Uso de depurador**:
      - Establecer un punto de interrupción en la línea `if x == 0x539`.
      - Ejecutar el script en modo depuración.
      - Cuando se alcance el punto de interrupción, evaluar `x * 2`.

   c) **Cálculo manual**:
      - Convertir 0x539 de hexadecimal a decimal: 1337.
      - Calcular 1337 * 2 = 2674.

3. **Verificar la solución**:
   - Confirmar que la flag es `flag{2674}`.

## Claves pedagógicas:

- ✅ **Comprensión de bucles**: Identificar y resolver problemas de bucles infinitos.
- ✅ **Depuración básica**: Uso de herramientas de depuración o modificación de código.
- ✅ **Sistemas numéricos**: Conversión entre hexadecimal y decimal.

## Variantes para incrementar la dificultad:

1. Usar una operación más compleja para generar la flag.
2. Ocultar la condición de salida en una función separada.
3. Implementar un contador de tiempo para limitar el tiempo de ejecución.

## Herramientas útiles:

- IDE con depurador (como PyCharm, VSCode)
- Python REPL para cálculos rápidos
- Calculadora con modo programador para conversiones hex/dec