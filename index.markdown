---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# Hello Actions World! - Informe

## Realización de hello-js-action-alu0101138558 

Enlace al repositorio: [hello-js-action-alu0101138558](https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558)

### Procedimientos realizados

Comencé el desarrollo de la práctica ejecutando los siguientes comandos: 

```bash 
git clone https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558.git
cd  hello-js-action-alu0101138558
echo "# Hello world javascript action" >> README.md
git add README.md
git commit -m "Creación del README.md"
git push -u origin master
```

Una vez clonado el repositorio `hello-js-action-alu0101138558` y realizado su primer **commit**, proseguí iniciando el repositorio para el desarrollo de una aplicación de node, por ello ejecuté lo siguiente:

```bash
npm init -y
```

Dando lugar al siguiente fichero `package.json` (la descripción se la añadí posteriormente)

```json
{
  "name": "hello-js-action-alu0101138558",
  "version": "1.0.0",
  "description": "Repo that´s contain hello js action",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558.git"
  },
  "keywords": [],
  "author": "Adrián Emilio Padilla Rojas <alu0101138558@ull.edu.es> (https://github.com/alu0101138558)",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558/issues"
  },
  "homepage": "https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558#readme",
}
```

Luego procedí a creación del fichero `action.yml`, este es de suma importancia en esta práctica, ya que contiene la acción que realizaremos posteriormente en el repositorio `hello-js-action-use-alu0101138558`. Dentro del fichero escribí lo siguiente:

```yml
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet:  # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'node12'
  main: 'index.js'
```

En lo que me fijé principalmente fue en `inputs` y `outputs`, ya que sus comportamientos serían modificados a través de nuestro programa establecido en `index.js`.

Para la realización de las distitntas acciones procedí a instalar de las herramientas `@actions/core` y `@actions/github`, haciendolo de la siguiente forma:

```bash
npm install @actions/core
npm install @actions/github
```

Dando lugar al directorio `node_modules` y al fichero `package-lock.json`.

Lo siguiente fue crear el fichero `index.js`, donde escribí el siguiente código: 

```js
const core = require('@actions/core');
const github = require('@actions/github');

try {
  // `who-to-greet` input defined in action metadata file
  const nameToGreet = core.getInput('who-to-greet');
  console.log(`Hello ${nameToGreet}!`);
  const time = (new Date()).toTimeString();
  core.setOutput("time", time);
  // Get the JSON webhook payload for the event that triggered the workflow
  const payload = JSON.stringify(github.context.payload, undefined, 2)
  console.log(`The event payload: ${payload}`);
} catch (error) {
  core.setFailed(error.message);
}
```

Lo que hace este programa es, obtener un nombre que se almacena en el `main.yml` (por defecto será **World**), mostrar la fecha del sistema cuando se ejecutó el proceso y, además, se imprimirá el evento **webhook** en el registro. Todo esto será visible en las **github actions**.

Por último, modifiqué el fichero `README.md` para que cualquier usuario pueda enterder como utilizar esta acción en su propio repositorio. El resultado es el siguiente: 

```md
# Hello world javascript action

## Description

This action prints "Hello World" or "Hello" + the name of a person to greet to the log.

## Inputs

### `who-to-greet`

**Required** The name of the person to greet. Default `"World"`.

## Outputs

### `time`
The time we greeted you.

## Example usage

uses: actions/hello-world-javascript-action@v6
with:
  who-to-greet: 'Mona the Octocat'
```

Para poder almacenar todos los cambios que hemos realiado hasta el momento en el repostorio, haremos lo siguiente: 

```bash
git add action.yml index.js node_modules/* package.json package-lock.json README.md
git commit -m "My first action is ready"
```
Y tambien le añadiremos una versión de la siguiente forma: 

```bash
git tag -a -m "My first action release" v1
git push --follow-tags
```

LLegados a este punto, me gustaría explicar un fallo que cometí con el versionado en la práctica, como iba siguiendo el tutorial paso a paso, no me dí cuenta de como debía nombrar a las versiones en caso de cometer algún error, es por ello que la versión actual del desarrollo de mi aplicación es la `6.0.0` en lugar de la `1.0.6`.

## Realización de hello-js-action-use-alu0101138558 

Enlace al repositorio: [hello-js-action-use-alu0101138558](https://github.com/ULL-ESIT-PL-2021/hello-js-action-use-alu0101138558)

### Procedimientos realizados

Para dar comienzo a esta parte, lo primero a tener en cuenta es que el repositorio `hello-js-action-alu0101138558` debe ser público, ya que si no fuese así, no podría ser incluido en el workflows del repositorio `hello-js-action-use-alu0101138558` (en mi caso era privado y lo modifiqué para que fuese público).

Lo siguiente, fue clonar el repositorio `hello-js-action-use-alu0101138558` y, estando en su interior, ejecutar los siguientes comandos:

```bash
mkdir .github
mkdir .github/workflows
```

Dentro del directorio workflows, cree fichero `main.yml` con la siguiente información:

```yml
name: Using hello world
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      # To use this repository's private action, you must check out the repository
      - name: Checkout
        uses: actions/checkout@v2
      - name: Hello world action step
        uses: ULL-ESIT-PL-2021/hello-js-action-alu0101138558@v6
        id: hello
        with:
          who-to-greet: 'Adrian Emilio Padilla Rojas'
      # Use the output from the `hello` step
      - name: Get the output time
        run: echo "The time was ${{ steps.hello.outputs.time }}" 
```

Una vez seguidos estos pasos, procedí a enviar los cambios al repositorio, para ver si todo había ido correctamente.

```bash
git add .
git commit -m "Modificacion v6"
git push origin master
```

Dando como resultado lo siguiente:

* En **Hello world action step**

![hello_world](img/hello_world.png)

* En **Get the output time**

![time](img/time.png)

## Realización de hello-js-action-use-alu0101138558 

Enlace al repositorio: [hello-js-action-super-alu0101138558](https://github.com/ULL-ESIT-PL-2021/hello-js-action-super-alu0101138558)

### Procedimientos realizados

En este repositorio se debe llevar a cabo lo que ya habíamos logrado en la práctica anterior, para ello ejecuté los siguientes comandos:

```bash
git clone https://github.com/ULL-ESIT-PL-2021/hello-js-action-super-alu0101138558.git
cd hello-js-action-super-alu0101138558
git submodule add https://github.com/ULL-ESIT-PL-2021/hello-js-action-alu0101138558.git
git submodule add https://github.com/ULL-ESIT-PL-2021/hello-js-action-use-alu0101138558.git
```

Dando lugar al fichero [.gitmodules](https://github.com/ULL-ESIT-PL-2021/hello-js-action-super-alu0101138558/blob/master/.gitmodules)