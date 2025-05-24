
## Relato de Execução – Exercício Final do Módulo Grunt

O exercício final do módulo de Grunt parecia simples à primeira vista, mas acabou se tornando um desafio maior do que o esperado, principalmente por dois motivos:

1. **O tempo espaçado entre os estudos** – Como estudei o conteúdo em blocos separados, acabei esquecendo algumas estruturas básicas.
2. **Erros pequenos que geraram grandes problemas** – A maior parte do tempo foi gasta resolvendo pequenos detalhes que impediam a execução correta do Grunt.

---

### Etapas do Exercício

#### 1. Criação da branch

Criei a branch `exercicio-grunt` com o comando:

```bash
git checkout -b exercicio-grunt
```

Esse passo foi simples, mas precisei revisar o comando com o GPT porque ainda não o utilizo com frequência.

#### 2. Criação do `Gruntfile.js`

A estrutura geral do `Gruntfile.js` estava correta, mas cometi um erro importante: coloquei os comandos `loadNpmTasks` e `registerTask` **fora da função `module.exports`**. Esse erro fez com que as tarefas não fossem reconhecidas e gerassem mensagens de que elas não estavam definidas.

Depois de muito revisar, percebi esse detalhe comparando com exemplos anteriores e corrigi a posição. A partir daí, o `npm run grunt` funcionou perfeitamente.

#### 3. Testes de compilação LESS

Criei uma alteração simples de `background-color` para garantir que o LESS estava sendo compilado corretamente e observei que o CSS foi devidamente minificado e funcional.

#### 4. Compressão com Uglify (JavaScript)

Ao configurar o `uglify`, escrevi `file` em vez de `files`, o que impedia a minificação sem apresentar erro direto. O JavaScript permanecia o mesmo e eu não entendia por quê. Mais uma vez, após revisar linha por linha, identifiquei o erro de digitação e corrigi.

Após isso, executei o `npm run build` e confirmei que o `main.min.js` foi gerado corretamente, com o código todo compactado em uma única linha.

#### 5. Reflexões sobre performance

Mesmo com uma pequena redução no tamanho do arquivo JS, entendi como isso impacta diretamente em projetos maiores, melhorando tempo de carregamento e consumo de dados – especialmente em redes móveis ou de baixa velocidade.

#### 6. Organização do repositório

Percebi que as pastas `dev/` e `dist/` estavam sendo versionadas por já terem sido comitadas antes. Corrigi isso excluindo essas pastas no GitHub, atualizando o `.gitignore` com:

```gitignore
node_modules/
dev/
dist/
```

#### 7. Deploy e finalização

Configurei corretamente os parâmetros da Vercel:

* **Build Command**: `npm run build`
* **Output Directory**: `dist`

Fiz o deploy e a aplicação funcionou perfeitamente em produção, com os arquivos minificados e separados corretamente.

---

### Considerações Finais

Esse exercício foi mais valioso do que aparentava. Não só revisei e fixei o uso do Grunt e seus plugins, como também entendi melhor a importância dos pequenos detalhes que fazem a diferença no desenvolvimento real. Percebi que anotar os passos durante o processo faz falta – e por isso, parei para registrar tudo ao final aqui.
