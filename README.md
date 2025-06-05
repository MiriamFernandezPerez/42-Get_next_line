# 📄 Get Next Line 
## *Lectura Eficiente Línea por Línea en C*

## 🎯 Propósito y Alcance

El proyecto `get_next_line` tiene como objetivo implementar una función en C que permita leer un archivo línea por línea de manera eficiente, sin necesidad de cargar todo el archivo en memoria. Esta función es especialmente útil para manejar archivos grandes o flujos de datos donde se requiere procesar una línea a la vez.

## 🧰 Descripción de la Biblioteca

La biblioteca proporciona una interfaz limpia que retorna una línea por llamada, gestionando internamente los búferes y la asignación de memoria. Ofrece dos implementaciones paralelas que comparten interfaces idénticas pero difieren en sus capacidades para manejar múltiples descriptores de archivo:

| Implementación | Archivos Involucrados                                         | Capacidad                                 |
|----------------|---------------------------------------------------------------|-------------------------------------------|
| Estándar       | `get_next_line.c`, `get_next_line.h`, `get_next_line_utils.c` | Un solo descriptor de archivo             |
| Bonus          | `get_next_line_bonus.c`, `get_next_line_bonus.h`, `get_next_line_utils_bonus.c` | Múltiples descriptores de archivo simultáneos |

## 🏗️ Arquitectura de Doble Implementación

La biblioteca emplea un enfoque de doble arquitectura donde los usuarios pueden elegir entre las implementaciones estándar o bonus según sus necesidades. Ambas implementaciones mantienen interfaces públicas idénticas a través de la función `get_next_line(int fd)`, asegurando compatibilidad directa para el código del cliente.

## 🔍 Componentes Principales y Entidades de Código

La funcionalidad de la biblioteca se basa en varias entidades clave que trabajan juntas para proporcionar la lectura línea por línea:

- **`get_next_line`**: Función principal que coordina las operaciones de lectura y procesamiento de líneas.
- **Funciones Utilitarias**: Encargadas de la manipulación de cadenas y la gestión de memoria.
- **Llamadas al Sistema**: Interacción con el sistema operativo para operaciones de lectura de archivos.

## ⚙️ Patrón de Operación Básico

La biblioteca sigue un patrón consistente para procesar la entrada de archivos y producir la salida línea por línea:

1. **Lectura**: Se lee del descriptor de archivo utilizando un búfer de tamaño configurable.
2. **Almacenamiento**: Se mantiene un estado persistente entre llamadas mediante búferes estáticos.
3. **Extracción**: Se extrae y retorna la siguiente línea disponible.
4. **Gestión de Memoria**: Se maneja automáticamente la asignación y liberación de memoria para las líneas retornadas.

Este patrón asegura un uso eficiente de la memoria mientras mantiene el estado persistente entre llamadas a la función mediante búferes estáticos.

## ✨ Características Clave

### 📌 Implementación Estándar

- Lectura línea por línea desde un solo descriptor de archivo.
- Tamaño de búfer configurable mediante la macro `BUFFER_SIZE`.
- Gestión automática de memoria con cadenas retornadas que deben ser liberadas por el cliente.
- Manejo de estado persistente entre llamadas a la función.
- Manejo de fin de archivo y errores mediante retornos de `NULL`.

### 🚀 Implementación Bonus

- Todas las características de la implementación estándar.
- Soporte para múltiples descriptores de archivo concurrentes.
- Gestión de estado independiente para hasta 1024 descriptores de archivo.
- Aislamiento de búferes por descriptor de archivo para seguridad en entornos multihilo.
- Interfaz de API idéntica para una integración sin problemas.

### 🧠 Gestión de Memoria

- Uso de arreglos de búferes estáticos para la persistencia del estado.
- Asignación dinámica para cadenas de líneas y operaciones temporales.
- Limpieza automática de recursos internos.
- Responsabilidad del cliente para liberar las cadenas de líneas retornadas.

### ⚙️ Configuración

- Configuración en tiempo de compilación del `BUFFER_SIZE` (por defecto: 42 bytes).
- Soporte para tamaños de búfer personalizados para optimizar el rendimiento.
- Compatibilidad con la biblioteca estándar de C.

## 📝 Guía de Uso

1. **Compilación**: Utiliza el `Makefile` proporcionado para compilar la biblioteca.
2. **Inclusión**: Incluye el archivo de encabezado correspondiente en tu proyecto.
3. **Uso**: Llama a la función `get_next_line(int fd)` pasando el descriptor de archivo deseado.
4. **Liberación de Memoria**: Asegúrate de liberar la memoria de las líneas retornadas para evitar fugas de memoria.

## 📎 Ejemplo de Uso

```c
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void) {
    int fd = open("archivo.txt", O_RDONLY);
    char *linea;

    if (fd == -1)
        return (1);
    while ((linea = get_next_line(fd)) != NULL) {
        printf("%s", linea);
        free(linea);
    }
    close(fd);
    return (0);
}
