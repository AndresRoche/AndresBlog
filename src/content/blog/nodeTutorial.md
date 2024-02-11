---
title: 'Tutorial NodeJS'
description: 'Tutorial para principiantes de como usar NodeJS'
pubDate: 'Feb 07 2024'
heroImage: '/Nodejs-cover.png'
---


# Curso de Node JS por parte de Midudev


1. [Instalación de manejador de paquetes para NodeJs.](#init)
2. [REPL De Node](#relp)
3. [ejecutar el Hola Mundo](#holaMundo)
4. [Variables Globales](#variablesGlobales)
5. [Patron de Diseño Módulos](#patronesDeDiseño)
    1. [Sistemas de módulos CommonJS -> cjs](#modulosCommonjs)
    2. [Sistemas de módulos ES Modules -> mjs](#ESmodules)
6. [Módulos Nativos de Nodejs](#modulosNativosDeNode)
    1. [OS](#OS)
    2. [FS](#FS)
        1. [Buffer](#buffer)
        2. [Asíncrono vs Síncrono](#asincronoVSSincrono)
        3. [Promises](#promises)
        4. [Transformar CallBacks a Promises](#callbacksAPromises)
        5. [Async Await](#asyncAwait)
        6. [Funciones Paralelas](#funcionesParalelas)
    3. [PATH](#path)
        1. [Hacer Un ls En node](#hacerUnLSEnNode)
    4. [Process](#process)
        1. [Volviendo con el ls](#volviendoConElLS)
7. [Primer Servidor Con NodeJs](#primerServidorConNodeJS)


## Instalación de manejador de paquetes para NodeJs <a name="init"></a>

En nuestro equipo podemos tener instaladas diferentes versiones de node js, para manejarlos todos usamos el manejador de paquetes Fnm.

1. Instalar el lenguaje Rust porque el fnm lo usa.

2. Instalar fnm para Windows.

3. Configurarlo en PowerShell es con este comando `fnm env --use-on-cd | Out-String | Invoke-Expression`, si usado otro terminal, revisa la documentación de [fnm](https://fnm.vercel.app)

Una vez descargado Fnm, instalaremos una versión de nodejs.

`fnm install 20.11.0`

Para ver nuestras versiones de node instaladas, usamos.

`fnm list` o `fnm ls`

Para usar por fin una de las versiones instaladas, es con:

`fnm use 20.11.0 `

Y ya lo estarías usando esa versión.

Antes de dejar el fnm usa el alias así.

`fnm alias 20.11.0 default`

Con esto hacemos que, por predeterminado, se use esa versión en diferentes terminales de comandos.


## REPL De Node <a name="relp"></a>

Al escribir en el terminal de comandos `node`, se nos abre el repl que es como la consola de las herramientas de desarrollo de los navegadores. También lenguajes como Python y Rust tienen su propio repl.

## ejecutar el Hola Mundo <a name="holaMundo"></a>

Creamos un archivo llamado index.js, así sería crearla con el terminal.

`touch index.js`

Y adentro del archivo colocamos un `console.log()`.

```javascript
console.log("Hola Mundo ♥");
```

Ahora, por fin ejecutaremos el código de esta manera es:

`node index.js`

Y nos da como salida:
![Hola mundo](/HolaMundo.PNG)

## OJO Variables Globales <a name="variablesGlobales"></a>

Si, vienes de JavaScript, sabrás que hay una variables llamada `window`, la cual no está disponible en node js. Si lo ejecuta, te dará error porque no existe.

Pero con el tiempo se hizo una nueva variable global, que está tanto en el navegador como en nodejs, la cual es `globalThis`

Ojo, con detalles, es cuando usando el `globalThis` en node apunta a una variable global llamada `global`, en cambio, cuando los usamos en navegador apunta a `windows`.

![GlobalThis](/globalThis.png)

Como nota, el `console.log()` en realidad proviene del `globalThis`, si escribieras `globalThis.console.log("hola mundo ♥")`, funcionaría.

## Patron de Diseño Módulos <a name="patronesDeDiseño"></a>

¿En nodejs es muy común ver el patrón de diseño de módulos, pero qué es? Simplemente, consiste en separar el código en módulos independientes y reutilizables. Este patrón de diseño en muy utilizado en programación orientada a objetos.


---


### Sistemas de módulos CommonJS -> cjs <a name="modulosCommonjs"></a>

Este sistema aún aparece en mucha documentación, pero no es recomendable el uso, porque hay mejores opciones.


Nombre del archivo : **sum.js**
```javascript
function sum (a, b) {
    return a + b
} 

module.exports = sum
```


Para exportar código se usa la variable global `module` en `module.export`, con esto ya estaríamos exportando la función sum.


Nombre del archivo : **index.js**
```javascript
const sum = require("./sum.js")

console.log(sum(5,5))
```


Para traernos sus funciones usamos el `require('')` y le indicamos en dónde se encuentra nuestro archivo.


Con este metodo Podemos tener problemas con el nombre por ejemplo:


Nombre del archivo : **index.js**
```javascript
const megustasouth = require("./sum.js")

console.log(megustasouth(5,5))
```

Aquí tenemos un problema: es que, podemos cambiar el nombre a la función "sum", aun así al ejecutar no dará error, todo funcionará, pero a la hora de la lectura no sabremos qué hace esa función. Para evitar esto, en el archivo **sum.js** hacemos lo siguiente:a


Nombre del archivo : **sum.js**
```javascript
function sum (a, b) {
    return a + b
} 

function resta (a, b) {
    return a - b
} 

module.exports = {
    sum,
    resta : resta

    //esto dos son equivalentes
    //para evitar escribir sum dos vecez solo escribimos sum y ya.
}
```


En `module.exports`, lo exportamos como un objeto de javascript, así nos obliga a destructurar el código en el **index.js**.


Nombre del archivo : **index.js**
```javascript
const {sum , resta} = require("./sum.js")

console.log(sum(5,5))
console.log(resta(5,5))
```
---

### Sistemas de módulos ES Modules -> mjs <a name="ESmodules"></a>

Esta es la forma más moderna de exportar, no solo en nodejs, sino también en JavaScript. Ahora podemos indicar que vamos a usar ES modules simplemente cambiando la extensión de 'js' por 'mjs'.


Nombre del archivo : **index.mjs**
```javascript
import { sum , resta } from './sum.js'

console.log(sum(5,5))
console.log(resta(5,5))

```
El `const {sum , resta} = require("./sum.js")` lo remplazamos por `import { sum , resta } from './sum.js'`.


Nombre del archivo : **sum.mjs**
```javascript
export function sum (a, b) {
    return a + b
} 

export function resta (a, b) {
    return a - b
} 

```

Aquí se nos quita el `module.export` y simplemente antes de función colocamos `export`.

---

## Módulos Nativos de Nodejs 🍱🦿🦾 <a name="modulosNativosDeNode"></a>

### OS 💻 <a name="OS"></a>

El os nos da información del sistema o PC.

```javascript
const os = require("node:os")   

console.log('Informacion del sistema operativo : ')
console.log('--------------------------------------------')
console.log('Nombre del sistema operativo', os.platform())
console.log('Version del sistema operativo', os.release())
console.log('Arquitectura', os.arch())
console.log('CPUs', os.cpus()) //<- vamos a poder escalar procesos de Node
console.log('Memoria libre', os.freemem() /1024 /1024) //para tenerlo em MB
console.log('Memoria total', os.totalmem() /1024 /1024) //para tenerlo em MB
console.log('Uptime', os.uptime() /60 /60) // el uptime lo da es segundos
```
Vamos por orden.
* Para usar ese módulo escribimos `const os = require("node:os")`
* `os.platform()` nos dice en qué sistema operativo estamos.
* `os.release()` La versión del sistema operativo.
* `os.arch()` arquitectura del sistema.
* `os.cpus()` nos muestra las CPUS que tenemos. Esto más adelante será importante, ya que nos permitirá escalar los programas de node.
* `os.freemen()` la memoria libre.
* `os.totalmen()` la memoria total.
* `os.uptime()` nos muestra cuánto tiempo lleva prendido el sistema.
---
### FS 📁 <a name="FS"></a>

FS es un módulo que permite la lectura y modificaciones de archivos. 

Para leer archivo es con **readFileSync** y **readFile**, Ejemplo:


Nombre del archivo : **archivo.txt**
```
Hola Mundo ♥
```

Nombre del archivo : **index.mjs**
```javascript
import fs from 'node:fs'

const text = fs.readFileSync('./archivo.txt', 'utf-8') 

console.log(text)
```

Como salida el programa estaría nuestro, "hola mundo ʕ•́ᴥ•̀ʔっ"
![holaMundo](/HolaMundo.PNG)

* Importamos `fs`, en este caso usé ES para importarlo.
* `fs.readFileSync('./archivo.txt', 'utf-8')` Con esta línea leemos el contenido, el archivo.
    * El primer parámetro es el nombre, el archivo.
    * Segundo parámetro indicamos que nos lo devuelva con la codificación 'utf-8'. Si no tuviera la codificación, nos devolvería un buffer. 
    ![buffer](/buffer.PNG)

#### Buffer?  <(' .' )>? <a name="buffer"></a>

El Buffer es una manera de transportar datos temporales y de un único uso. Esto se diseñó originalmente para que las computadoras no se queden sin datos durante la transferencia de datos irregulares o por las velocidades de los procesadores.

#### Asíncrono vs Síncrono <a name="asincronoVSSincrono"></a>

En Nodejs el código puede ejecutarse de manera Síncrono o Asíncrono.


Estos serán nuestros archivos a leer.

Nombre del archivo : **archivo.txt**
```
ʕ•́ᴥ•̀ʔっ hola mundo ♥ (っ◔◡◔)っ ❤
```

Nombre del archivo : **archivo2.txt**
```
Ultrakill 4-4 clair de soleil
Ultrakill OST Versus
The Death of god s will full
big balls Scratch21 (AC/DC Cover)
Minos prime Singing My Way
Night Dance pero la Canta los Perfectos Desconocidos
```

**Síncrono** 

Nombre del archivo : **index.js**
```javascript
const fs = require("node:fs")

console.log('leyendo el Primer Archivo...')

const text1 = fs.readFileSync('./archivo.txt', 'utf-8')

console.log(text1, '\n')

console.log('leyendo el Segundo Archivo...')

const text2 = fs.readFileSync('./archivo2.txt', 'utf-8')

console.log(text2)
```

Salida:

![Síncrono](/lercturaSincrono.PNG)

Como vemos en el código, NodeJS sigue la secuencia que establecimos.
* Primero ejecuta el `console.log('leyendo el Primer Archivo...')`.
* Luego, lee el archivo y, **cuando termina de leerlo, ejecuta el `console.log(text1, '\n')`**.
* Para luego ir con el segundo archivo.

Todo bien, pero, tenemos un pequeño problema: es cuando al leer el archivo, detiene la ejecución principal del programa y no sigue hasta que el archivo termine de leerse.

Esto es un problema porque el tiempo que gasta leyendo el archivo se puede ejecutar, otras tareas, y esto es prioritario para los servidores, que es en lo que más se usa NodeJS.

Entonces, ¿cómo ejecutamos el código de forma asíncrona?


**Asíncrono** 
Nombre del archivo : **index.js**
```javascript
const fs = require("node:fs")

console.log('\n\nleyendo el Primer Archivo...')
fs.readFile('./archivo.txt', 'utf-8', (err, text) => {
    console.log(text, '\n')
})


console.log(5+ 1+ 3 /4 * 5)

console.log('leyendo el Segundo Archivo...')
fs.readFile('./archivo2.txt', 'utf-8', (err, text) => {
    console.log(text, '\n')
})
```

Salida:

![Asincrono](/lercturaAsincrono.PNG)

El `fs.readFile` es igual al `fs.readFileSinc`, pero en vez de retornar el texto se agrega un CallBacks. La función espera que el archivo cargue por completo y cuando termine ejecutará el callback. Esto lo hace gracias a que crea un hilo separado del hilo principal.


Así mantenemos la ejecución principal de nodejs intacta.

****



### Promises <a name="promises"></a>

Siguiendo con la asincronía, podemos adaptar el código a promesas de JavaScript, para eso, en vez de requerir `node:fs` usamos `node:fs/promises`.

Nombre del archivo : **index.js**
```javascript
const fs = require("node:fs/promises")

console.log('\n\nleyendo el Primer Archivo...')
fs.readFile('./archivo.txt', 'utf-8').then( text => {
    console.log(text, '\n')
})

console.log(5+ 1+ 3 /4 * 5)

console.log('leyendo el Segundo Archivo...')
fs.readFile('./archivo2.txt', 'utf-8').then( text => {
    console.log(text, '\n')
})
```

La salida no cambia solo la forma de escribirlo.


### Transformar CallBacks a Promises <a name="callbacksAPromises"></a>

Ahora, puede ser que el `node:fs/promises` no tenga alguna función o método, que no esté en forma de promesas. Para esas ocasiones podemos convertirlo en promesas de la siguiente forma.

Supongamos que el método `readFile` no tenga una version en promesas, osea que no usemos `node:fs/promises`.

Nombre del archivo : **index.js**
```javascript
const fs = require("node:fs")
const { promisify } = require('node:util')

const readFilePromise = promisify(fs.readFile)

console.log('\n\nleyendo el Primer Archivo...')
readFilePromise('./archivo.txt', 'utf-8').then( text => {
    console.log(text, '\n')
})

console.log(5+ 1+ 3 /4 * 5)

console.log('leyendo el Segundo Archivo...')
readFilePromise('./archivo2.txt', 'utf-8').then( text => {
    console.log(text, '\n')
})
```

* OJO con el detalle que estamos requiríendo `node:fs` y no `node:fs/promises`.
* Importamos el método `promisify` de `node:util`.
* `const readFilePromise = promisify(fs.readFile)` Aquí, creamos el nuevo , pero con promesas.



### Async Await <a name="asyncAwait"></a>

Siguiendo con `node:fs/promises`, también está disponible el Async Await de JavaScripft. Pero si tiene la extensión `.js` se **debe usarlo adentro de una función o dará error**.


Este código no funcionara ❌❌❌❌❌❌.

Nombre del archivo : **index.js**
```javascript
const { readFile } = require('node:fs/promises')

console.log('\n\nleyendo el Primer Archivo...')
const text = await readFile('./archivo.txt', 'utf-8')

console.log(text, '\n')

console.log(5+ 1+ 3 /4 * 5)

console.log('leyendo el Segundo Archivo...')
const texto2 = await readFile('./archivo2.txt', 'utf-8')

console.log(texto2, '\n')

```
Este código no funcionara ❌❌❌❌❌❌.

No funcionará debido a que el async await sí o sí debe usarse dentro de una función, como:

Nombre del archivo : **index.js**
```javascript
const { readFile } = require('node:fs/promises')

// IIFE - Inmediatly Invoked Function Expression

;(
    async () => {
        console.log('\n\nleyendo el Primer Archivo...')
        const text = await readFile('./archivo.txt', 'utf-8')

        console.log(text, '\n')

        console.log(5+ 1+ 3 /4 * 5)

        console.log('leyendo el Segundo Archivo...')
        const texto2 = await readFile('./archivo2.txt', 'utf-8')

        console.log(texto2, '\n')
    }
)()
```

Aquí, estamos creando una función anónima que, tan pronto se crea, se usa. Ojo, si quieres usar esto, recuerda siempre empezarlos con un `;()` para evitar errores.

Si quieres usar el async await, pero no en funciones, también puedes cambiar la extensión de `.js` a `.mjs` 

Nombre del archivo : **index.mjs**
```javascript
import { readFile } from "node:fs/promises";

console.log("\n\nleyendo el Primer Archivo...");
const text = await readFile("./archivo.txt", "utf-8");

console.log(text, "\n");

console.log(5 + 1 + (3 / 4) * 5);

console.log("leyendo el Segundo Archivo...");
const texto2 = await readFile("./archivo2.txt", "utf-8");

console.log(texto2, "\n");

```

#### Funciones Paralelas <a name="funcionesParalelas"></a>

Con esto podemos hacer que se hagan dos procesos a la vez y que no se interrumpa el hilo principal del node. 

Nombre del archivo : **index.mjs**
```javascript
import { readFile } from "node:fs/promises";

Promise.all([
    readFile('./archivo.txt', 'utf-8'),
    readFile('./archivo2.txt', 'utf-8')
]).then( ([text, secundText]) => {
    console.log(text);
    console.log(secundText);
})
```

Ojo, no será como en las promesas o callbacks que, tan pronto que termine de cargar, se ejecuta. aquí los `console.log` siempre se va a ejecutar en orden.


### PATH 💻 <a name="path"></a>

Path nos permite revisar las carpetas del dispositivo. Ojo con detalles: para usar path, hay que tener muy en cuenta si, el sistema operativo es Windows o Linux, porque en Windows las carpetas se usan con `\`, pero en Linux se une el otro `/`.

Nombre del archivo : **index.js**
```javascript
const path = require('node:path')

console.log(path.sep)

const filePath = path.join('content', 'subfolder', 'text.txt')
console.log(filePath);

const base = path.basename('/tmp/mideu/password.txt')
console.log(base);

const filename = path.basename('/tmp/mideu/password.txt', '.txt')
console.log(filename);

const ext = path.extname('/tmp/mideu/img.hoo.jpg')
console.log(ext);
```


Salida:

![path](/path.PNG)

Aquí, tenemos algunos métodos.
* `path.sep` Nos muestra la separación de las rutas del sistema operativo, o sea, si usa `/` o `\`.
* `path.join` Este nos une las rutas de acuerdo al sistema operativo. Como estoy en Windows, me las une así: `content\subfolder\text.txt`.
* `path.basename` El nombre, del archivo junto a su extensión.
* `path.extname` Nos da la extensión de un archivo.


### Hacer Un ls En node. <a name="hacerUnLSEnNode"></a>

Como pequeño reto, vamos a hacer un ls.

```javascript
const fs = require("node:fs")

fs.readdir('.', (err , files) => {
    if(err){
        console.log("Error en el directorio ,", err);
    }

    files.forEach( file => {
        console.log(file);
    })
})
```

* Una cosa que se me olvidó comentar en los callbacks es que siempre va primero el error y luego tu respuesta, así para que no se olviden de programar los errores.


### Process <a name="process"></a>

`Process` es una variable global que nos permite ver los procesos por los que pasa la ejecución de node.

Vamos a ver algunas cosas de `process` para luego volver al `ls` y mejorarlo.

Ejecución del archivo:`node .\index.js Hola que hace`


```javascript
// argumentos de entrada
console.log(process.argv)

// current working directory
console.log(process.cwd())

// platform
console.log(process.env.PEPITO)

// podemos controlar eventos del proceso
process.on('exit', () => {
    console.log("Te salistes XD");
})

// controlar el proceso y su salida
process.exit(1)
```


Salida:

![process](/process.PNG)

* `process.argv` Nos muestra los argumentos de entrada. Como vemos, aparte de colocar el clásico `node .\index.js`, colocamos más argumentos de entrada que podemos leerlos.
* `process.cwd()` Muestra en qué directorio estamos ejecutando el script. Puedes probar en irte un directorio atrás (`cd ..`) y ejecutar el archivo desde ahí.
* `process.env.PEPITO`, Son las variables de entornos, para que funcionen, si o sí, hay que **ejecutarlo en Bash**, no en CMD o PowerShell de Windows porque nos dará error, y sería así para ejecutarlos `PEPITO=hola node 7.process.js`.
* `process.on` También podemos marcarle eventos. En este caso es un evento cuando salga el programa.
* `process.exit(1)` Detiene la ejecución del programa. Si le pasamos como parámetro `0` es que funcione correctamente el programa y si lo pasamos como `1`, es que hubo algún error.

### Volviendo con el ls. <a name="volviendoConElLS"></a>

Para esta mini aplicación hemos reunido todos los conocimientos de atrás.

```javascript
const fs = require("node:fs/promises")
const path = require("node:path")

const folder = process.argv[2] ?? '.'

async function ls (folder){
    let files
    try {
        files = await fs.readdir(folder)
    } catch (error) {
        console.log("NO se puedo encontrar el directorio");
        process.exit(1)
    }

    const filesPromise = files.map( async file => {
        let stats

        try {
            stats = await fs.stat(path.join(folder, file))
        } catch (error){
            console.log("Error Al extraer datos de los archivos");
            process.exit(1)
        }
        
        const isDirectory = stats.isDirectory()
        const fileType = isDirectory ? 'd' : 'f'
        const fileSize = stats.size.toString()
        const fileModificate = stats.mtime.toLocaleString()

        return `${fileType} ${file.padEnd(20)} ${fileSize.padStart(10)} ${fileModificate}`
    })

    const fileInfo = await Promise.all(filesPromise)

    fileInfo.forEach(info => {
        console.log(info);
    });
}


ls(folder)
```

* Usamos los argumentos de entrada `const folder = process.argv[2] ?? '.'`.
* Las funciones async await.



<!-- aqui empiaaz toda una explicación de que es npm y las dependecias... etc, eso creo que voy hacer otro aparte -->



## Primer Servidor Con NodeJs <a name="primerServidorConNodeJS"></a>

Podemos crear un servidor en NodeJS. Hay paquetes de NPM que lo hacen más fácil, pero esto es un tutorial de NodeJS.

```javascript
const http = require("node:http")

const server = http.createServer((req, res) => {
    console.log("request received")
    res.end('Hola mundo')
})

server.listen(0, () => {
    console.log(`server listening on port http://localhost:${server.address().port}`);
})
```

* Primero requerimos de `node:http` para la creación del servidor.
* `const server = http.createServer((req, res) => {* Aquí creamos el servidor, pero no a qué puerto escucha.
* `server.listen(0, () => {` Aquí, le indicamos el puerto así. En dónde va el `0` es donde le indicamos el puerto, pero aquí le colocamos 0 para que nos busque automáticamente un puerto disponible. Y luego para saber qué puerto estamos usando `server.address().port`.