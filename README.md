# Módulo 18 - Grunt

## Menu
[Aula 1 - Configuração Grunt](#aula-1---configuração-grunt)  
[Aula 2 - Crie tarefas](#aula-2---crie-tarefas)  
[Aula 3 - ](#aula-)  
[Aula 4 - ](#aula-)  
[Aula 5 - ](#aula-)  
[Aula 6 - ](#aula-)  
[Aula 7 - ](#aula-)  


## Aula 1 - Configuração Grunt

### Objetivos da aula

* Compreender o propósito e os benefícios da ferramenta **Grunt**;
* Instalar o Grunt globalmente e localmente;
* Criar o arquivo de configuração `Gruntfile.js`.

---

### Etapas iniciais

#### 1. Acessar a pasta do projeto

Navegue até a pasta onde deseja configurar o projeto e abra o terminal nela.

#### 2. Instalar o Grunt CLI globalmente

Execute no terminal:

```bash
npm i -g grunt-cli
```

Isso instala o `grunt-cli`, que é a interface de linha de comando do Grunt.

**O que é o `grunt-cli`?**
O `grunt-cli` permite que você use o comando `grunt` no terminal. Ele apenas chama o Grunt instalado localmente no seu projeto, e por isso precisa ser instalado globalmente.

#### 3. Inicializar o projeto com `npm init`

Execute:

```bash
npm init
```

Esse comando criará o `package.json`. Basta responder às perguntas como nome, versão, ponto de entrada, autor (pode usar seu nome, ex: *Matheus*), etc.

#### 4. Adicionar o script do Grunt ao `package.json`

Dentro da seção `"scripts"`, adicione:

```json
"grunt": "grunt"
```

Lembre-se de separar corretamente com vírgulas se houver outros scripts como `"test"`.

---

### Configuração do Grunt

#### 5. Criar o arquivo `Gruntfile.js`

Esse arquivo controla todas as tarefas automatizadas do projeto. Estrutura básica:

```js
module.exports = function(grunt) {
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json')
    });
};
```

* `module.exports`: Exporta uma função que recebe o objeto `grunt`;
* `grunt.initConfig`: Inicializa a configuração;
* `pkg: grunt.file.readJSON('package.json')`: Carrega os dados do `package.json` para uso nas tarefas.

**Sobre o `grunt.file.readJSON`:**
Essa função importa os dados do `package.json`, como nome, versão e outras informações úteis dentro das configurações. Embora pareça uma abordagem um pouco mais antiga, ainda funciona bem para projetos simples e didáticos como esse.

#### 6. Executar o Grunt

Com tudo configurado, execute:

```bash
npm run grunt
```

Como ainda **não existe uma tarefa padrão definida**, o Grunt será executado, mas encerrará logo em seguida. Assim como o Gulp, o Grunt precisa de uma tarefa `default` para funcionar corretamente.

## Aula 2 - Crie tarefas

### Objetivos da aula

* Criar tarefas personalizadas usando o Grunt;
* Entender a ordem de execução das tarefas no Grunt;
* Simular tarefas demoradas e assíncronas.

---

### Criando a primeira tarefa personalizada

Após configurar o `Gruntfile.js`, criamos uma tarefa personalizada com o método `grunt.registerTask`. A estrutura utilizada foi:

```js
grunt.registerTask('olaGrunt', function() {
    console.log('Olá Grunt');
});
```

* `'olaGrunt'` é o nome da tarefa;
* A função anônima define o comportamento da tarefa;
* Dentro da função, usamos `console.log('Olá Grunt')` para imprimir a mensagem no terminal.

Para executar essa tarefa, usamos no terminal:

```bash
npm run grunt olaGrunt
```

---

### Simulando uma tarefa assíncrona

Para simular uma tarefa demorada, foi utilizado `setTimeout`, representando ações que levam tempo (como leitura de arquivos ou chamadas de rede):

```js
grunt.registerTask('olaGrunt', function() {
    const done = this.async();

    setTimeout(function() {
        console.log('Olá Grunt');
        done();
    }, 3000);
});
```

* `this.async()` informa ao Grunt que a tarefa será concluída manualmente, e retorna uma função (`done`) que será chamada quando a tarefa terminar;
* Dentro do `setTimeout`, usamos `done()` após o `console.log` para indicar o fim da tarefa;
* Com isso, o Grunt espera os 3 segundos antes de encerrar a execução.

---

### Definindo uma tarefa padrão (default)

Para evitar a necessidade de escrever o nome da tarefa toda vez no terminal, podemos definir uma tarefa padrão com:

```js
grunt.registerTask('default', ['olaGrunt']);
```

* `'default'` é o nome reservado para a tarefa padrão no Grunt;
* O segundo argumento é um array com o nome das tarefas que devem ser executadas por padrão — no caso, apenas `'olaGrunt'`.

Agora, basta executar:

```bash
npm run grunt
```

E o Grunt executará automaticamente a tarefa `'olaGrunt'`, exibindo "Olá Grunt" após o tempo definido.

---

