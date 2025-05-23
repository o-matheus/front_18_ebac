# Módulo 18 - Grunt

## Menu
[Aula 1 - Configuração Grunt](#aula-1---configuração-grunt)  
[Aula 2 - Crie tarefas](#aula-2---crie-tarefas)  
[Aula 3 - Use o grunt para compilar o less ](#aula-3--use-grunt-para-compilar-less)  
[Aula 4 - Execute tarefas de forma paralela com grunt ](#aula-4--execute-tarefas-de-forma-paralela-com-grunt)  
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

## Aula 3 – Use Grunt para compilar LESS

### Objetivos da Aula
- Instalar e configurar o plugin LESS no Grunt.
- Criar tarefas de compilação para ambientes de desenvolvimento e produção.
- Utilizar o Grunt para compilar e automatizar tarefas com LESS.

### Instalação do Plugin LESS
Certifique-se de estar na pasta do projeto e rode o comando:
```bash
npm install --save-dev grunt-contrib-less
```

### Carregando o Plugin no Grunt
Antes de modificar a tarefa default, carregue o plugin com:
```js
grunt.loadNpmTasks('grunt-contrib-less');
```

### Configuração do LESS no initConfig
Adicione a configuração do LESS no `grunt.initConfig`:
```js
less: {
  development: {
    files: {
      'main.css': 'main.less'
    }
  }
}
```

### Criando o Arquivo de Origem
Crie um arquivo `main.less` no projeto. Ele será compilado automaticamente para `main.css` ao rodar a tarefa LESS.

### Atualizando a Tarefa Default
Substitua a tarefa default para usar o LESS:
```js
grunt.registerTask('default', ['less']);
```

### Executando o Grunt
Rode o Grunt com o comando:
```bash
npm run grunt
```
Isso cria corretamente o `main.css` a partir do `main.less`.

### Ambientes de Execução no Grunt
O uso de `development` indica que o Grunt está configurado para ambiente de desenvolvimento. Também é possível configurar o ambiente de produção, que costuma incluir compressão de arquivos.

#### Configuração de Produção com Compressão
```js
less: {
  production: {
    options: {
      compress: true
    },
    files: {
      'main.min.css': 'main.less'
    }
  }
}
```

### Trabalhando com SASS no Grunt

#### Instalação do Plugin
```bash
npm install --save-dev grunt-contrib-sass
```

#### Carregando o Plugin
```js
grunt.loadNpmTasks('grunt-contrib-sass');
```

#### Configuração no initConfig
```js
sass: {
  dist: {
    options: {
      style: 'compressed'
    },
    files: {
      'main.min.css': 'main.scss'
    }
  }
}
```

#### Tarefa Default com LESS e SASS
```js
grunt.registerTask('default', ['less', 'sass']);
```

### Observações Importantes
- Sempre que um bloco de código não for o último dentro de `initConfig`, adicione uma vírgula ao final.
- LESS usa `options: { compress: true }` para comprimir.
- SASS usa `options: { style: 'compressed' }` para comprimir.

Perfeito! Abaixo está a **Aula 4 – Execute tarefas de forma paralela com Grunt**, estruturada exatamente no mesmo estilo e formatação da Aula 3 que você compartilhou:

---

## Aula 4 – Execute tarefas de forma paralela com Grunt

### Objetivos da Aula

* Compreender as limitações da execução serial de tarefas no Grunt.
* Instalar e configurar o plugin `grunt-concurrent`.
* Executar múltiplas tarefas simultaneamente para melhorar o desempenho da automação.

### Problemas da Execução Serial

No Grunt, tarefas são executadas em série por padrão. Isso significa que mesmo tarefas independentes precisam aguardar a conclusão da anterior para iniciar. Em projetos com múltiplas tarefas, isso pode causar lentidão desnecessária.

### Instalação do Plugin concurrent

Certifique-se de estar na pasta do projeto e rode o comando:

```bash
npm install --save-dev grunt-concurrent
```

### Carregando o Plugin no Grunt

Carregue o plugin adicionando ao `Gruntfile.js`:

```js
grunt.loadNpmTasks('grunt-concurrent');
```

### Configuração do concurrent no initConfig

Adicione a configuração do plugin no `grunt.initConfig`:

```js
concurrent: {
  target: ['less', 'uglify']
}
```

> Substitua `'less'` e `'uglify'` pelas tarefas que você deseja executar em paralelo.

### Atualizando a Tarefa Default

Atualize a tarefa default para usar o concurrent:

```js
grunt.registerTask('default', ['concurrent']);
```

### Executando o Grunt

Rode o Grunt com o comando:

```bash
npm run grunt
```

Isso executa em paralelo todas as tarefas definidas dentro de `concurrent.target`.

### Exemplo Completo com LESS e Uglify

```js
grunt.initConfig({
  less: {
    development: {
      files: {
        'main.css': 'main.less'
      }
    }
  },

  uglify: {
    build: {
      files: {
        'main.min.js': ['main.js']
      }
    }
  },

  concurrent: {
    target: ['less', 'uglify']
  }
});
```

### Observações Importantes

* O bloco `concurrent` permite que tarefas independentes rodem ao mesmo tempo.
* É fundamental separar blocos com vírgulas dentro do `initConfig`.
* Use `grunt.registerTask('default', ['concurrent'])` para definir a tarefa padrão com execução paralela.
* Ideal para tarefas como `less`, `sass`, `uglify`, entre outras, que não precisam esperar umas pelas outras.

