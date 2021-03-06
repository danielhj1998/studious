---
layout: note
title: Mapa de memoria del PLC
---

En el caso de los PLC de Delta, se tiene un mapa de memoria, el mapa de memoria completo se puede ver para cada tipo de PLC, en este caso se ocupará el de SS2 y [aquí está su mapa de memoria](http://www.deltronics.ru/images/manual/DVP-ES2-EX2-SS2-SA2-SX2-SE-TP_PM_EN_20181030.pdf#page=35).

Se cuentan con 2 tipos de almacenamientos, de 1 bit y de palabra (16 bit).

# Entradas/Salidas
Las entradas `X` y las salidas `Y` son de 1 bit y se cuentan con 37 puertos de 8 bit, por lo que se pueden tener 256 entradas/salidas máximo. Claro que para esto se requeriría comprar un puerto de expansión dependiendo del tipo de PLC.

| Símbolo | Tipo              | Rango                 |
| -       | -                 | -                     |
| `X`     | Entradas internas | `X0` - `X377` (octal) |
| `Y`     | Salidas internas  | `Y0` - `Y377` (octal) |

# Relés auxiliares
Estos se ocupan para almacenar datos estados (tipo booleano) en un programa, su marca es `M` y existen de 3 tipos:

| Tipo      | Rango             |
| -         | -                 |
| General   | `M0` - `M511`     |
|           | `M768` - `M999`   |
|           | `M2000` - `M2047` |
| Enclavado | `M512` - `Y767`   |
|           | `M248` - `M4095`  |
| Especial  | `M1000` - `M1999` |

Los de uso general son los que el usuario puede cambiar dependiendo de sus necesidades. Los enclavados son sólamente de lectura y los especiales (de los cuales algunos son sólamente de lectura) sirven al usuario para tener conocimiento sobre el estado del procesador, detectar errores, etc.

# Temporizadores
Se cuentan con temporizadores de hasta $100$, $10$ y $1 ms$, los cuales son accesibles a través de las marcas `T`. En bobinas se lee su valor y con el bloque `TMR` se inicia y configura el temporizador. Además, algunos de los temporizadores de $100$ y de $10 ms$ son convertibles a $10$ y $1 ms$ si se activan ciertos relés virtuales (`M1028` y `M1038` respectivamente).

> Todas las etiquetas que se muestran en el documentos, son reconocidas por el software ISPSoft.

# Comparadores
Se pueden realizar comparaciones por medio del bloque de *comparison contact*. Algunas de las operaciones de comparación más importantes son:

| Símbolo | Descripción |
| :-:     | -           |
| `&`     | AND         |
| `|`     | OR          |
| `^`   | XOR                          |
| `INV` | NOT                          |
| `=`   | Igual que                    |
| `>`   | Mayor que                    |
| `<`   | Menor que                    |
| `<>`  | Diferente de                 |
| `>=`  | Mayor o igual que            |
| `<=`  | Menor o igual que            |
| `DX`  | Para datos de 32 bit         |
| `FX`  | Para datos de punto flotante |

# Movimiento de datos `MOV`
La instrucción `MOV`, asociada al mismo bloque de instrucción. Copia datos a un registro. Estos registros pueden ser formados como una unión de marcas internas de la siguiente forma:

| Nombre  | Descripción                                                      |
| -       | -                                                                |
| `K1M10` | 1 Nibble a partir de la marca `M10`, es decir: `M10,M11,M12,M13` |
| `K2M10` | 2 Nibble a partir de la marca `M10`, es decir: `M10 - M17`       |
| `K3M10` | 3 Nibble a partir de la marca `M10`, es decir: `M10 - M21`       |

Así se puede hacer referencia a registros compuestos por varias marcas internas.

Usando un bloque `MOV`, se puede guardar una seria de datos en un registro para su utilización posterior.

Existe las distintas variantes de `MOV` tienen que ver con el tipo de dato, pero también con la forma de activación. Existe la versión `MOVP` que trabaja con flancos. Esto sirve para contadores, señales de reloj, etc. También existe un elemento de línea que hace que se detecten sólo los flancos de subida o bajada. Al usar estos, es necesario utilizar las instrucciones con `P` al final.
