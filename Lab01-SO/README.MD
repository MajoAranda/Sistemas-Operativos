# Informe lab1 Sistemas operativos

## Introducción

Los usuarios de sistemas operativos usan como herramienta fundamental la terminal o línea de comandos. Este trabajo consistió en implementar una herramienta más simple pero en esencia la misma. Entonces al completar este programa logramos entender a mayor profundidad una herramienta tan importante para nuestra tarea como desarrolladores de software.

## Estructura del proyecto

El código fuente del proyecto está compuesto de diferentes módulos que se encargan de tareas específicas y que se comunican entre sí para lograr que el programa funcione correctamente. Los principales módulos son `scommand/pipeline`, `mybash`, `parsing`, `builtin` y `execute`.

### Módulo scommand/pipeline

Dado la gramática del shell y su división en 3 capas, decidimos utilizar la estructura de datos `GSList` de la librería `glib 2.0` para la implementación del TAD que representa cada comando. Con esto pudimos abstraernos de la implementación de lista de cadenas al usar las funciones de la librería y poder concentrarnos en implementar cada función para completar los TADs que representan a comandos simples y líneas de tuberías.

### Módulo parsing

En la implementación del parsing usamos las funciones declaradas en `parser.h` para poder detectar argumentos de entrada.

### Módulo builtin

Módulo que implementa los comandos internos. Estos son 3: `cd`, `help` y `exit`.
Utilizamos las funciones del módulo `scommand/pipeline` y de librerías del lenguaje C para escribir estas implementaciones. Al ser comandos internos no fue necesario usar syscall `fork`. Directamente usamos syscalls como `chdir` y `exit` para lograr la funcionalidad. En una primera versión se logró correr el programa pero tenía bugs y para solucionarlo usamos funciones de librería como `strcmp()`. Luego también se añadió que se pueda cambiar directorios usando argumentos como `.` o `..` para completar la funcionalidad.

Para implementar nuevos comandos internos es sencillo ya que simplemente usamos un arreglo de cadenas para guardar cada comando y luego de verificar que se recibió un comando válido se lo compara para determinar cual es y correr el código requerido para ejecutar dicho comando.

### Módulo exec

Este módulo se encarga de invocar a las syscalls necesarias para ejecutar los comandos ingresados por consola. Nos basamos fuertemente en la modularización para hacer más legible y ordenado el código.

Consiste en una función principal, `execute_pipeline` que verifica si el pipeline se debe ejecutar en foreground o background. En el caso de que se ejecute en segundo plano, la función `fork()` crea un proceso hijo para manejar el pipeline completo. El hijo redirige las entradas y salidas estándar si es necesario mediante las funciones `change_fd_in` y `change_fd_out`. Luego, ejecuta el pipeline de comandos mediante la función `execute_foreground`, que ejecuta el pipeline dependiendo si el pipeline contiene uno o múltiples comandos con las respectivas funciones `single_command_pipeline` y `multiple_commands`. A su vez, estas funciones diferencian entre comandos internos y externos.

## Puntos estrella

- Se generalizó para pipelines de más de dos comandos simples.

## Conclusión

Con todo el trabajo realizado durante este laboratorio logramos comprender mucho mejor como funciona un programa tan importante como bash. También aprendimos sobre llamadas a sistema como `fork`, `execvp`, etc. Esto lo logramos poco a poco con la ayuda de los profesores tanto del teórico como del taller práctico. También al googlear y usar el manual de man pages. Fue un proyecto sumamente interesante el cual teníamos 1 enunciado con tareas y objetivos claros y al mismo tiempo libertad de decisiones. Adquirimos experiencia en trabajar en equipo y programación a bajo nivel.
