# Computaci-n-cient-fica-2-
Profe acá voy a poner los talleres y lo que mandes tal vez quede raro porque es desde el celular 



primer taller

import collections

def resolver_subsecuencia_balanceada(n, a):
    """
    Calcula la longitud de la subsecuencia balanceada no decreciente más larga.

    Args:
        n (int): La longitud del arreglo.
        a (list): El arreglo de enteros.

    Returns:
        int: La longitud de la subsecuencia más larga.
    """
    if n == 0:
        return 0

    # 1. Caso: Sub-secuencia con un solo valor distinto.
    # Es la máxima frecuencia de cualquier elemento (es balanceada por definición).
    frecuencias = collections.Counter(a)
    max_len = 0
    if frecuencias:
        max_len = max(frecuencias.values())

    # Obtenemos los valores únicos ordenados
    valores_unicos = sorted(frecuencias.keys())
    
    # Mapeo de valor a una lista de sus índices de ocurrencia
    indices_por_valor = collections.defaultdict(list)
    for i, val in enumerate(a):
        indices_por_valor[val].append(i)

    # 2. Caso: Sub-secuencia con dos valores distintos v1 < v2.
    # Debe ser de la forma [v1, ..., v1, v2, ..., v2] donde count(v1) == count(v2).
    # Intentamos maximizar la longitud 2 * c.
    
    for i in range(len(valores_unicos)):
        v1 = valores_unicos[i]
        
        for j in range(i + 1, len(valores_unicos)):
            v2 = valores_unicos[j]
            
            # Los indices deben ser i1 < i2 < ... < ic < j1 < j2 < ... < jc
            
            indices1 = indices_por_valor[v1]
            indices2 = indices_por_valor[v2]
            
            c = 0 # Contador para la cantidad de pares (v1, v2)
            idx2_ptr = 0 # Puntero para los índices de v2
            
            for idx1 in indices1:
                # Buscar el primer índice de v2 que sea mayor que el índice actual de v1
                while idx2_ptr < len(indices2) and indices2[idx2_ptr] <= idx1:
                    idx2_ptr += 1
                
                # Si encontramos un v2 después de v1
                if idx2_ptr < len(indices2):
                    # Podemos formar un par (v1 en idx1, v2 en indices2[idx2_ptr])
                    c += 1
                    # "Consumimos" el índice de v2 para la siguiente iteración
                    idx2_ptr += 1
                
                # Si hemos agotado las ocurrencias de v2, salimos
                if idx2_ptr >= len(indices2):
                    break

            # La longitud máxima de la forma [v1, ..., v2] donde count(v1) == count(v2) es 2 * c
            max_len = max(max_len, 2 * c)

    return max_len

# --- Lectura de la entrada (basado en el formato del ejemplo) ---
import sys
import io

def leer_casos_prueba(input_data):
    # La entrada usa un formato donde el número de casos T no está explícito en el archivo,
    # sino que se deduce. Asumiremos que el archivo tiene N y luego A.
    
    # Configuramos la entrada para leer desde la cadena proporcionada.
    if isinstance(input_data, str):
        sys.stdin = io.StringIO(input_data)
        
    resultados = []
    
    # Intentamos leer hasta que no haya más entrada
    while True:
        try:
            # Leer N
            linea_n = sys.stdin.readline().strip()
            if not linea_n:
                break
            n = int(linea_n)
            
            # Leer el arreglo A
            linea_a = sys.stdin.readline().strip().split()
            if not linea_a:
                break
            a = [int(x) for x in linea_a]
            
            resultados.append(resolver_subsecuencia_balanceada(n, a))
            
        except EOFError:
            break
        except Exception as e:
            # print(f"Error reading input: {e}", file=sys.stderr)
            break
            
    return resultados

# Datos del ejemplo proporcionado en la imagen (asumiendo que es la entrada completa)
entrada_ejemplo = """
5
1 4 4 4 4
2
1 2
9
1 1 1 2 2 2 3 3 3
15
1 1 1 2 2 2 3 3 3 4 4 4 5 5 5
"""

# Ejecutar el código con la entrada de ejemplo
resultados_ejemplo = leer_casos_prueba(entrada_ejemplo.strip())

# Impresión de la salida esperada
print("Salida:")
for res in resultados_ejemplo:
    print(res)
