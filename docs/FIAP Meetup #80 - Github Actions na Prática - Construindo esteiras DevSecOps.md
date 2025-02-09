# FIAP Meetup #80 - Github Actions na Prática - Construindo esteiras DevSecOps

> ref: https://www.youtube.com/watch?v=kiBsfvrsqw8

## Github Actions

> ref: https://github.com/features/actions

Automações que são criadas para diversos fins, como analisar código, dependencias, segurança, build, entre outros.

É possível criar pipelines com mais de um job.

Na prática, uma máquina chamada runner executará a série de ações predefinida por nós.

É gratuito, mas possui limites, pois rodam na como maquinas virtuais na azure.

## Workflows

Configurações para rodar os jobs, devem ficar na pasta `.github/workflows` e são escritos em arquivos .yaml

## Repositório

> ref: https://github.com/magnologan/nodejs-goof

Neste projeto utilizaremos como referencia o `magnologan/nodejs-goof`, um projeto propositalmente com algumas vulnerabilidades e erros.

## Detalhando o workflow

```yaml
name: GitHub Actions Demo #nome da action
on: [push] #quando será executada
jobs: #sequencia de passos que serão executados
  Explore-GitHub-Actions: #job
    runs-on: ubuntu-latest #em que tipo de máquina será executada
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4 # automações públicas e terceiras (user/repository)
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

Todos os "run" são comandos sendo executados.

Todos os comandos são executados a partir do arquivo yaml e não podem ser interativos, se precisar de um resultado diferente, o arquivo yaml deve ser alterado novamente.

## Github actions

> ref: https://github.com/actions/checkout

Este repositorio é bastante comum, possui etapas que copiam o repositório atual para dentro de uma maquina virtual para que as validações sejam iniciadas.

As actions podem ser executadas simnultaneamente ou podem seguir um fluxo.

## Sobre o runners

Essas máquinas virtuais que rodam na Azure são preparadas para realizarem tarefas de desenvolvimento, se executado o comando `run env` será possível ver uma série de variáveis que podem exemplificar diversas ferramentas na máquina. Dentre as variáveis, há a gitgub token, um token que permite que a máquina faça alterações no repositório.

Os runners podem executar containers e outras funções que permitem deixar autmações mais robustas.

### Self hosted runners

São runners criados por você ou sua organização geralmente para trabalhos mais sensíveis ou que envolvem regulamentação. Utilizando esse tipo de abordagem, evita-se compartilhamento de informações com serviços terceiros.

## Github Marketplace

> ref: https://github.com/marketplace

Local onde podemos buscar por actions já prontas, são desenvolvidas e disponibilizadas por terceiros. Há diversas categorias, entre elas algumas de segurança, deployment, qualidade de código, etc.

## Snyk

> ref: https://github.com/marketplace/actions/snyk

Plataforma de segurança voltada para desenvolvedores que analisa componentes e dependencias para verificar se há vulnerabilidades. SCA: _Software composition analysis_.

Ao acessar snyk.io, é possível criar um token para uso nas actions.

O secret gerado, pode ser utilizado em settings/secrets/actions do repositório e deve ter o mesmo nome da variário utilizada na action.

Os resultados do scanner pode aparecer tanto no github actions quando serem enviados diretamente para o snyk.io, onde podemos ver o resultado em forma de relatório.

Também é possívle ver as dependencias diretas, indiretas e até realizar correções de vulnerabilidades ou ignora-las.

É possível utilizar o snyk para analisar repositórios sem a necessidade de passar por um action.

## Workflow Dispatch

Dentre os eventos que podem disparar uma action está o workflow_dispatch, ao utiliza-lo o workflow pode ser executado manualmente através de um botão que será disponibilizado pelo github.

## ZAP Scan

> ref: https://github.com/marketplace/actions/zap-baseline-scan

Permite rodar scans de vulnerabilidades dinamicos na aplicação, DAST. No marketplace é possível encontrar mais de um tipo de Zap Scan. Utilizaremos o baseline.

Para este tipo de analise, será necesario que a aplicação esteja rodando, este repositório não considera este cenário, por isso será realizada uma analise em uma outra aplicação (https://juice-shop.herokuapp.com).

O zap Scan gera um relatório ao final do action e também pode incluir itens na guia issues do repositório.

## DevSecOps

Neste projeto rápido, aplicamos de forma simples algumas técnicas de DevSecOps para analise de código e aplicação através de actions, rotinas automatizadas preparadas para buscar vunerabilidades que precisam ser corrigidas.
