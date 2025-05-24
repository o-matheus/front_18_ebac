# Módulo 18 - Grunt

## Menu
[Aula 1 - Configuração Grunt](#aula-1---configuração-grunt)  
[Aula 2 - Crie tarefas](#aula-2---crie-tarefas)  
[Aula 3 - Use o grunt para compilar o less ](#aula-3--use-grunt-para-compilar-less)  
[Aula 4 - Execute tarefas de forma paralela com grunt ](#aula-4--execute-tarefas-de-forma-paralela-com-grunt)  
[Aula 5 - Iniciar um projeto com o grunt ](#aula-5--iniciar-um-projeto-com-o-grunt)  
[Aula 6 - Observar mudanças com o grunt com o watch ](#aula-6--observe-mudanças-com-o-grunt-com-o-watch)  
[Aula 7 - Comprime HTML com Grunt ](#aula-7--comprime-html-com-grunt)  
[Aula 8 - Conheça a JavaScript Math ](#aula-8--conheça-a-javascript-math)  
[Aula 9 - Comprime JavaScript com o Grunt ](#aula-9--comprime-javascript-com-o-grunt)  


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

Claro! Abaixo está o **texto unificado da Aula 5 – Iniciar um projeto com o Grunt**, seguindo exatamente o padrão do último README que você aprovou (como fizemos com a Aula 4), pronto para você usar como `DALL·E5` no seu projeto:

---

## Aula 5 – Iniciar um projeto com o Grunt

### Objetivos da Aula

* Organizar a estrutura de um projeto completo com Grunt.
* Separar ambientes de desenvolvimento e produção.
* Compilar arquivos CSS utilizando o pré-processador LESS.
* Utilizar variáveis e importar fontes externas para melhorar a manutenção e o estilo do CSS.

### Estrutura Inicial do Projeto

Começamos criando a base do projeto, com um foco prático na construção de uma aplicação simples: um **sorteador de números**. A estrutura foi organizada da seguinte forma:

```
/projeto
├── Gruntfile.js
├── package.json
├── .gitignore
└── src/
    ├── index.html
    ├── styles/
    │   └── main.less
    └── script/
        └── (arquivos .js)
```

A pasta `src` representa o código-fonte em desenvolvimento. Dentro dela, `styles/` será usada para arquivos LESS e `script/` para os arquivos JavaScript. A estrutura ainda contará com as pastas `dev/` e `dist/` para os resultados gerados em cada ambiente.

### Estrutura do HTML

No arquivo `index.html`, criamos:

* Uma tag `<main>` com o título principal (`<h1>Sorteador</h1>`).
* Um formulário `<form>` com:

  * Um `<label>` indicando "Número máximo".
  * Um `<input type="number" id="numero-maximo">`.
  * Um `<button type="submit">Sortear número</button>`.

Também foram importadas duas fontes do Google Fonts: **Roboto** (fonte principal) e **Lobster** (fonte dos títulos).

### Variáveis no LESS

Para tornar o CSS mais modular e reutilizável, criamos variáveis no arquivo `main.less` utilizando a sintaxe do LESS:

```less
@fontePrincipal: 'Roboto', sans-serif;
@fonteTitulo: 'Lobster', cursive;
@corTexto: #fff;
@corDeFundo: #b3b5aa;
```

Essas variáveis foram aplicadas no `body`, controlando fontes e cores do projeto com facilidade.

### Configuração do LESS para múltiplos ambientes

Atualizamos a configuração no `Gruntfile.js` para suportar dois ambientes:

#### Desenvolvimento:

```js
less: {
  development: {
    files: {
      'dev/styles/main.css': 'src/styles/main.less'
    }
  }
}
```

#### Produção:

```js
less: {
  production: {
    options: {
      compress: true
    },
    files: {
      'dist/styles/main.min.css': 'src/styles/main.less'
    }
  }
}
```

### Scripts no package.json

Adicionamos um script no `package.json` para facilitar a execução da versão de produção:

```json
"scripts": {
  "build": "grunt build"
}
```

Agora, para gerar a versão minificada, basta executar:

```bash
npm run build
```

### Atualização no HTML para ambientes

Durante o desenvolvimento, usamos o CSS da pasta `dev`:

```html
<link rel="stylesheet" href="./dev/styles/main.css">
```

Na produção, usamos a versão comprimida da pasta `dist`:

```html
<link rel="stylesheet" href="./dist/styles/main.min.css">
```

### Considerações Finais

Esse projeto nos permite enxergar, na prática, a importância de separar ambientes de desenvolvimento e produção, mantendo o código organizado e eficiente. Além disso, o uso de variáveis no LESS e a configuração correta das pastas de entrada e saída tornam o workflow mais profissional e sustentável. Em breve, também configuraremos o plugin `watch` para automatizar ainda mais o processo de build e detectar alterações em tempo real.

Entendi perfeitamente, Matheus! Nesse caso, a tarefa `less:development` **não** está diretamente na tarefa `default`, e sim **dentro da configuração do `watch`**, que é chamada na `default`. Com isso, a recompilação do LESS só acontece **quando o `watch` detectar alterações**.

Aqui está o **texto corrigido e finalizado da Aula 6**, no mesmo padrão das aulas anteriores, pronto para ser usado como `DALL·E6`:

---

## Aula 6 – Observe mudanças com o Grunt, com o Watch

### Objetivos da Aula

* Instalar e configurar o plugin de observação `watch` no Grunt.
* Entender como funciona o monitoramento de arquivos com o Grunt.
* Automatizar o fluxo de trabalho recompilando automaticamente os arquivos LESS.

---

### Instalação do Plugin

Para utilizar o recurso de observação, instalamos o plugin `grunt-contrib-watch` com o seguinte comando:

```bash
npm install --save-dev grunt-contrib-watch
```

---

### Carregando o Plugin no Grunt

Após instalar, carregamos o plugin no `Gruntfile.js` utilizando:

```js
grunt.loadNpmTasks('grunt-contrib-watch');
```

---

### Configuração do Watch no initConfig

Após a configuração do `less`, adicionamos o bloco `watch` dentro do `grunt.initConfig`:

```js
watch: {
  less: {
    files: ['src/styles/**/*.less'],
    tasks: ['less:development']
  }
}
```

#### Explicação:

* `files`: define os arquivos que serão monitorados. O padrão `src/styles/**/*.less` significa:

  * `**/` observa todas as subpastas dentro de `styles`;
  * `*.less` observa todos os arquivos `.less`.
* `tasks`: define quais tarefas devem ser executadas quando uma alteração for detectada. Aqui, executamos `less:development`.

---

### Atualizando a Tarefa Default

Diferente das aulas anteriores, a tarefa `default` **não executa diretamente o `less:development`**. Ela apenas inicia o `watch`, que por sua vez cuida da recompilação automática sempre que um arquivo LESS for alterado:

```js
grunt.registerTask('default', ['watch']);
```

Assim, ao executar o comando `npm run grunt`, o Grunt começa a monitorar os arquivos `.less`. Quando uma modificação é detectada, ele automaticamente recompila o CSS de acordo com a configuração de desenvolvimento.

---

### Testando o Watch na Prática

Durante a aula, foi feito um teste alterando o tamanho da fonte do `<h1>` no arquivo `main.less`. Logo após salvar o arquivo, a tarefa `watch` identificou a alteração e executou a recompilação automática, atualizando o CSS em `dev/styles/main.css` sem a necessidade de rodar `npm run build`.

---

### Como parar o Watch no terminal

Para interromper a execução contínua do `watch` e liberar o terminal, basta utilizar o seguinte atalho:

```
CTRL + C
```

Perfeito, Matheus! Aqui está o **texto unificado da Aula 7 – Comprime HTML com Grunt**, com tudo o que você relatou, no mesmo padrão fiel e organizado dos seus READMEs anteriores (`DALL·E1` a `DALL·E6`). Sem complementos extras, seguindo somente o que foi realmente abordado na aula:

---

## Aula 7 – Comprime HTML com Grunt

### Objetivos da Aula

* Otimizar o código HTML de um projeto web usando o Grunt.
* Configurar e usar o plugin `grunt-replace`.
* Configurar e usar o plugin `grunt-contrib-htmlmin` para minificação de HTML.

---

### Estilizações no LESS e no HTML

Durante a aula, criamos variáveis para a cor de fundo e a cor do texto dos botões, com o objetivo de facilitar manutenções futuras e reutilizações. Após os botões, continuamos as alterações no seletor `input`, adicionando propriedades como `display: block`, `margin: 16px auto 0 auto`, `font-size: 2em`, `max-width: 120px`, `border: none`, `background-color: transparent`, `text-align: center`, `color: #fff` e `border-bottom: 6px solid #fff`.

Além disso, passamos a trabalhar com dois arquivos HTML: um `index.html` de desenvolvimento, apontando para `main.css`, e outro `index.html` de produção, que será gerado com o caminho atualizado para o CSS minificado, `main.min.css`.

---

### Configuração do plugin `grunt-replace`

Instalamos o plugin com:

```bash
npm install --save-dev grunt-replace
```

Após isso, carregamos o plugin no `Gruntfile.js` com:

```js
grunt.loadNpmTasks('grunt-replace');
```

Configuramos a tarefa `replace`, com a opção `patterns`, onde definimos o `match` como `ENDERECO_DO_CSS` e o `replacement` como o caminho do CSS. No HTML, usamos `@@ENDERECO_DO_CSS` como marcador. A versão de produção usou como base o mesmo padrão da configuração de `dev`, apenas substituindo as pastas envolvidas (de `dev` para `dist`) e o nome do arquivo CSS de `main.css` para `main.min.css`.

---

### Minificação do HTML com `grunt-contrib-htmlmin`

Instalamos o plugin com:

```bash
npm install --save-dev grunt-contrib-htmlmin
```

E carregamos no Grunt:

```js
grunt.loadNpmTasks('grunt-contrib-htmlmin');
```

No `initConfig`, configuramos o `htmlmin` com a opção `removeComments` e `collapseWhitespace`. O arquivo minificado é gerado a partir de `src/index.html` e salvo em uma pasta temporária chamada `prebuild`. O código HTML fica todo em uma linha só, comprovando a minificação.

---

### Ajuste da tarefa `replace` para produção

Reaproveitamos a estrutura do `replace` de desenvolvimento, alterando a pasta de origem para `prebuild` e a de destino para `dist`, além de ajustar o caminho do CSS minificado. Com isso, o arquivo final de produção (`dist/index.html`) passa a apontar corretamente para `main.min.css`.

---

### Exclusão da pasta temporária com `grunt-contrib-clean`

Após o processo de build, é necessário remover a pasta temporária `prebuild`. Para isso, instalamos o plugin:

```bash
npm install --save-dev grunt-contrib-clean
```

E carregamos no `Gruntfile.js` com:

```js
grunt.loadNpmTasks('grunt-contrib-clean');
```

Configuramos o `clean` diretamente com um array, sem usar chaves, apenas listando a pasta `prebuild` que deve ser apagada após a finalização:

```js
clean: ['prebuild']
```

Na tarefa `build`, adicionamos o `clean` logo após `replace:dist`, garantindo que a pasta temporária seja excluída automaticamente ao fim do processo.

---

### Atualização do `watch` para acompanhar HTML

Além de observar os arquivos LESS, agora configuramos o `watch` para acompanhar também mudanças no `index.html`. Adicionamos um novo bloco `html` dentro da configuração de `watch`, monitorando `src/index.html` e executando `replace:dev` sempre que houver alterações.

Com isso, ao rodar o comando:

```bash
npm run grunt
```

o Grunt passa a acompanhar mudanças tanto no LESS quanto no HTML, recompilando os arquivos automaticamente conforme as edições. Ao testar uma alteração no texto do `h1`, o sistema reconheceu a mudança e atualizou corretamente.

---

Com isso, encerramos a configuração completa da otimização e automação de HTML no projeto, concluindo a Aula 7.

Perfeito! Aqui está o **texto unificado da Aula 8 – Conheça a JavaScript Math**, fiel às suas anotações, estruturado como os outros READMEs (`DALL·E1` a `DALL·E7`) e pronto para ser incluído no seu projeto como `DALL·E8`:

---

## Aula 8 – Conheça a JavaScript Math

### Objetivos da Aula

* Integrar JavaScript ao projeto com foco em geração de números aleatórios.
* Utilizar `Math.random()` em conjunto com `Math.floor()` e `Math.round()`.
* Trabalhar com `document.getElementById`, `innerText`, `querySelector` e eventos.
* Evitar o recarregamento automático do formulário com `event.preventDefault()`.
* Esconder e revelar mensagens dinamicamente com CSS e JS.
* Garantir valores válidos com validação de formulário no HTML.

---

### Preparação do HTML e Exibição do Resultado

Criamos uma `div` com a classe `resultado` abaixo do formulário. Dentro dela, adicionamos o parágrafo:

```html
<p>O número sorteado foi: <span id="resultado-valor"></span></p>
```

Esse `span` será usado para exibir o número sorteado com JavaScript. Inicialmente, o valor era mostrado via `alert`, mas agora usamos:

```js
document.getElementById('resultado-valor').innerText = numeroAleatorio;
```

---

### Estruturação do JavaScript

Criamos o arquivo `main.js` dentro da pasta `src/script/` e o incluímos no HTML com:

```html
<script src="@@ENDERECO_DO_JS"></script>
```

> O caminho real do JS é injetado pela tarefa `replace` do Grunt.

No JS, garantimos que o código só será executado após o carregamento completo do HTML:

```js
document.addEventListener('DOMContentLoaded', function() {
  // código JS
});
```

---

### Captura do Formulário e Envio

Adicionamos a id `form-sorteador` ao formulário e escutamos o evento de envio com:

```js
document.getElementById('form-sorteador').addEventListener('submit', function(event) {
  event.preventDefault(); // evita recarregamento
});
```

Capturamos o valor do input com:

```js
let numeroMaximo = document.getElementById('numero-maximo').value;
numeroMaximo = parseInt(numeroMaximo);
```

---

### Geração do Número Aleatório

A fórmula utilizada para gerar o número aleatório foi:

```js
let numeroAleatorio = Math.floor(Math.random() * numeroMaximo) + 1;
```

> A soma de `+1` impede que o número `0` seja sorteado, garantindo um intervalo válido a partir de `1`.

Além do `Math.floor()`, também conhecemos outras duas funções:

* `Math.ceil()`: arredonda para cima.
* `Math.round()`: arredonda para o inteiro mais próximo.

Cada uma pode ser usada conforme o comportamento desejado.

---

### Validação no HTML

Para evitar o erro ao sortear sem digitar um número, adicionamos ao input os atributos:

```html
<input type="number" id="numero-maximo" required min="2">
```

* `required`: impede envio sem valor.
* `min="2"`: impede valores inválidos como `1`, que resultaria em sorteio único.

---

### Estilização do Resultado com LESS

No arquivo `main.less`, adicionamos estilo à `div.resultado`:

```less
.resultado {
  padding: 16px;
  margin-top: 120px;
  border: 8px dotted #fff;
  display: none;
}
```

E ao `span` dentro dela:

```less
.resultado span {
  font-weight: bold;
  font-size: 3em;
  display: block;
}
```

Inicialmente, a `div.resultado` está oculta. Quando o número é sorteado, ela é exibida com:

```js
document.querySelector('.resultado').style.display = 'block';
```

> Utilizamos `querySelector` e referenciamos a classe com ponto (`'.resultado'`).

A `div.resultado` foi movida para dentro da `<main>` para manter a organização visual e evitar quebra de layout.

---

Com isso, encerramos a aula com a integração completa do JavaScript ao projeto, exibindo dinamicamente o número sorteado, validando entradas e melhorando a experiência do usuário com estilo e interatividade.

Claro! Abaixo está o **texto unificado da Aula 9 – Comprime JavaScript com o Grunt**, pronto para ser salvo como `DALL·E9`, no mesmo padrão dos seus READMEs anteriores. Tudo foi organizado com base apenas nas suas anotações, sem adição de conteúdo extra:

---

## Aula 9 – Comprime JavaScript com o Grunt

### Objetivos da Aula

* Instalar e configurar o plugin `grunt-contrib-uglify` para minificação de arquivos JavaScript.
* Automatizar a substituição do caminho do JS no HTML para produção.
* Integrar a minificação de scripts à tarefa `build` do Grunt.
* Organizar corretamente as pastas do projeto no `.gitignore`.
* Realizar o deploy da aplicação na **Vercel**, com separação de ambiente de desenvolvimento e produção.

---

### Instalação e Configuração do Uglify

Começamos a aula instalando o plugin responsável por minificar os arquivos JavaScript:

```bash
npm install --save-dev grunt-contrib-uglify
```

Depois da instalação, adicionamos no `Gruntfile.js` o carregamento do plugin:

```js
grunt.loadNpmTasks('grunt-contrib-uglify');
```

E configuramos o `uglify` no `initConfig`, logo após o `clean`:

```js
uglify: {
  target: {
    files: {
      'dist/scripts/main.min.js': 'dist/scripts/main.js'
    }
  }
}
```

---

### Atualização da Tarefa `build`

Incluímos a tarefa `uglify` na sequência do `build`:

```js
grunt.registerTask('build', [
  'less:production',
  'htmlmin:dist',
  'replace:dist',
  'uglify',
  'clean'
]);
```

Ao rodar:

```bash
npm run grunt build
```

O arquivo `main.js` foi minificado corretamente para `main.min.js`, reduzindo cerca de 200 bytes e ficando com o conteúdo em uma única linha, sem espaços ou quebras.

---

### Atualização do Replace para Produção

Assim como já feito com o CSS, foi necessário atualizar o `replace:dist` para apontar o caminho correto do novo arquivo minificado de JavaScript:

```js
{
  match: 'ENDERECO_DO_JS',
  replacement: 'dist/scripts/main.min.js'
}
```

Essa substituição garante que o `index.html` final de produção utilize o JS comprimido e otimizado.

---

### Organização do Repositório no Git

Foi observado que pastas como `dev/` e `dist/` **não deveriam ter sido versionadas**. Como elas já tinham sido comitadas anteriormente, não bastava adicioná-las ao `.gitignore`.

Para resolver:

* As pastas `dev/` e `dist/` foram **apagadas diretamente no repositório remoto** (ex: GitHub).
* O `.gitignore` foi atualizado com:

```gitignore
node_modules/
dev/
dist/
```

Isso garantiu que essas pastas passassem a ser ignoradas nos próximos commits.

---

### Deploy na Vercel

Na etapa final, fizemos o deploy da aplicação utilizando a **Vercel**. Durante o processo de importação do repositório na plataforma, realizamos duas configurações fundamentais em **Build & Output Settings**:

1. **Build Command**:

   ```
   npm run build
   ```

2. **Output Directory**:

   ```
   dist
   ```

Essas configurações garantem que a Vercel use corretamente os arquivos do ambiente de produção gerados pelo Grunt.

Após o push final e a configuração, o projeto foi publicado com sucesso. A aplicação está no ar com arquivos otimizados e separados corretamente entre desenvolvimento e produção.
