Afya Admin — Dashboard com Blazor WebAssembly e MudBlazor

Identificação

Aluno(a): MAGGIO HENRIQUE VALENTE LOBO

Matrícula: 42004104-72

Faculdade: AFYA SÃO LUCAS PORTO VELHO - UNIDADE II

Curso: CIÊNCIA DA COMPUTAÇÃO

Disciplina: PROGRAMAÇÃO PARA SISTEMAS WEB

Professor(a): LILUYOUD CURY DE LACERDA

Semestre: 2026.2

Objetivo do projeto

Desenvolver um painel administrativo com dados fictícios utilizando Blazor WebAssembly e MudBlazor, praticando a criação de componentes, temas e layouts responsivos sem adicionar CSS próprio.

Tecnologias utilizadas

.NET 10

Blazor WebAssembly

MudBlazor 9

C# e Razor

Git e GitHub

Como executar

É necessário ter o Git e o SDK .NET 10 instalados.

git clone https://github.com/Maggio128/afya-admin.git
cd afya-admin
dotnet watch --project .\afya-admin.csproj --no-launch-profile --urls http://localhost:5051

Após aparecer a mensagem Now listening on, acessar:

http://localhost:5051/

Manter o terminal aberto enquanto utiliza a aplicação.

Progresso

Projeto base compilando e página inicial funcionando.

Repositório publicado no GitHub.

Pastas bin/ e obj/ excluídas do versionamento.

Tema, layout e componentes do dashboard serão implementados nas próximas etapas.

Dificuldades e soluções

Muitas dificuldades com sintaxes de comandos e execução do dashboard por falta de experiência e conhecimento dos motivos de falhas na execução.

Problemas que posso explicar:

Falha ao iniciar o servidor porque a porta já estava ocupada.

Erro de estrutura XML ao colocar um bloco fora do elemento Project no arquivo .csproj.

Página inicial retornando 404 porque ainda não existia uma página com a rota /.

Erro ao executar git push antes de criar o primeiro commit.

Telas

## Telas

### Tema claro
![Dashboard em tema claro](docs/prints/tema-claro.png)

### Tema escuro
![Dashboard em tema escuro](docs/prints/tema-escuro.png)

### Versão mobile
![Dashboard no celular](docs/prints/mobile.png)

### Inspeção do HTML
![HTML no DevTools](docs/prints/devtools.png)

### Estrutura do projeto
```
afya-admin/
├── Components/
│   ├── AtividadesRecentes.razor
│   ├── CabecalhoPagina.razor
│   ├── DashboardCard.razor
│   ├── GraficoDistribuicaoClientes.razor
│   ├── GraficoReceita.razor
│   ├── KpiCard.razor
│   ├── PerformanceProjetos.razor
│   ├── ProjetosRecentes.razor
│   ├── SeletorPeriodo.razor
│   └── Ui.cs
├── Data/
│   └── DashboardData.cs
├── Layout/
│   ├── MainLayout.razor
│   └── NavMenu.razor
├── Pages/
│   ├── Dashboard.razor
│   └── NotFound.razor
├── Properties/
│   └── launchSettings.json
├── docs/
│   └── prints/
│       ├── tema-claro.png
│       ├── tema-escuro.png
│       ├── mobile.png
│       └── devtools.png
├── wwwroot/
│   ├── css/
│   │   └── app.css
│   ├── img/
│   │   └── alex-morgan.jpg
│   └── index.html
├── _Imports.razor
├── App.razor
├── Program.cs
├── afya-admin.csproj
├── .gitignore
└── README.md
```

Components: componentes reutilizáveis e funções auxiliares da interface.

Data: modelos e dados fictícios utilizados no Dashboard.

Layout: estrutura compartilhada, incluindo barra superior e menu lateral.

Pages: páginas associadas às rotas da aplicação.

Properties: configuração dos perfis de execução.

docs/prints: capturas usadas na documentação.

wwwroot: arquivos públicos, como HTML, imagens e estilos do modelo.

## Componentes criados

![Tabela de componentes, responsabilidades e parâmetros](docs/prints/componentes.png)


O que aprendi

1. Como uma aplicação Blazor WebAssembly inicia no navegador? Qual é o papel do index.html, da <div id="app"> e do Program.cs?

O navegador abre o wwwroot/index.html, a única página HTML real do projeto, que tem a <div id="app"> com uma animação de carregamento. O script blazor.webassembly.js baixa o runtime do .NET (em WebAssembly) e as DLLs do projeto, e então o runtime executa o Program.cs. Nele, a linha builder.RootComponents.Add<App>("#app") manda renderizar o componente App dentro da #app, o que substitui a animação. O App.razor tem o roteador, que olha a URL e escolhe a página com o @page correspondente (no meu caso o Dashboard.razor), renderizando-a dentro do MainLayout. O AddMudServices() do Program.cs registra os serviços de que os componentes do MudBlazor precisam.

2. Qual é a diferença entre um Layout, uma Page e um Component neste projeto? Dê um exemplo de cada.

O Layout é a moldura que envolve todas as páginas, com o ponto @Body onde o conteúdo entra; no projeto é o MainLayout.razor, que contém a AppBar, o sidebar e o tema. A Page responde a uma URL por ter a diretiva @page; o exemplo é o Dashboard.razor (@page "/"), que apenas monta os componentes. O Component é uma peça reutilizável que recebe dados por parâmetros e não tem rota; o exemplo é o KpiCard.razor, que uso quatro vezes, uma para cada indicador.

3. O que é um RenderFragment e como o DashboardCard usa esse recurso para ser reutilizado por vários cards?

Um RenderFragment é um parâmetro que recebe um pedaço de marcação (HTML e outros componentes), funcionando como um "buraco" que quem usa o componente preenche. O DashboardCard tem a estrutura fixa do card (fundo, título, menu "⋮") e três buracos: Acoes (algo à direita do título), Menu (itens do menu "⋮", que só aparece se for informado) e ChildContent (o conteúdo principal, o que fica entre as tags). Por isso cinco blocos diferentes usam a mesma moldura: o GraficoReceita preenche as três partes, enquanto o AtividadesRecentes preenche só o Menu e o ChildContent. Sem isso eu teria de repetir a marcação do card cinco vezes.

4. Como funciona o @bind-Valor no SeletorPeriodo? Qual é o papel do ValorChanged?

O Blazor tem uma convenção: se um componente tem um parâmetro Valor e um EventCallback chamado ValorChanged, quem o usa pode escrever @bind-Valor="variavel". Isso passa o valor da variável para dentro do componente e, quando o componente dispara o ValorChanged, atualiza a variável de quem o usa. No SeletorPeriodo, ao clicar numa opção, o método chama ValorChanged.InvokeAsync(opcao). O componente não altera o próprio Valor, só avisa; quem atualiza é a página: no Dashboard.razor, @bind-Valor="_periodo" guarda a escolha em _periodo, e o novo valor volta como parâmetro e muda o texto do botão. O dono do estado é a página.

5. Por que os dados ficam na pasta Data, separados dos componentes? Que vantagem isso traz se, no futuro, os dados vierem de uma API?

A pasta Data responde "o quê mostrar" (números, nomes, cores), no DashboardData.cs, e a Components responde "como mostrar" (layout, gráficos, tipografia). Os componentes não têm dados escritos neles: recebem tudo por parâmetros, como Projetos, Atividades e Segmentos. Se os dados vierem de uma API, só muda a origem (a página buscaria na API e passaria o resultado), e os componentes continuam iguais, porque não sabem de onde os dados vêm. Também deixa o Dashboard.razor curto e fácil de manter.

6. Como o MudGrid com xs, sm e lg faz os cards de KPI se reorganizarem em telas de tamanhos diferentes?

O MudGrid divide a largura em 12 colunas, e cada MudItem diz quantas colunas ocupa em cada tamanho de tela; o valor vale "daquele tamanho para cima". Nos KPIs usei xs="12" sm="6" lg="3": no celular (menos de 600px) cada card ocupa 12 colunas, ou seja, um por linha; no tablet (a partir de 600px) ocupa 6, ou seja, dois por linha; no desktop (a partir de 1280px) ocupa 3, ou seja, quatro por linha. Quando a janela muda de tamanho, o grid recalcula sozinho, sem CSS meu.

7. Como foi possível estilizar a página inteira sem escrever CSS? Explique o papel do tema (MudTheme) e das classes utilitárias.

Usei três ferramentas do MudBlazor. Os parâmetros dos componentes (Elevation, Variant, Color, Size, Typo) controlam boa parte do visual. O tema (MudTheme), no MainLayout.razor, concentra as paletas clara e escura, o arredondamento, a altura da AppBar e a fonte Inter; os componentes leem as cores de variáveis CSS geradas a partir dele, então mudar o tema muda tudo junto, e o modo escuro funciona definindo a PaletteDark e alternando o IsDarkMode. As classes utilitárias, como pa-4, d-flex, flex-grow-1, align-center, mud-text-secondary e rounded-lg, já vêm no MudBlazor.min.css. Por isso não criei nenhum .razor.css nem mexi no app.css.

8. Por que o namespace do projeto é afya_admin e não afya-admin?

O .NET usa o nome do projeto como namespace raiz, mas o hífen não é permitido em identificadores do C#: afya-admin seria lido como a subtração "afya menos admin". Por isso o SDK troca o hífen por sublinhado. A pasta, o .csproj e o bundle de estilos continuam com hífen, mas no código C# e Razor vale o sublinhado: using afya_admin; no Program.cs e @using afya_admin.Layout no _Imports.razor. Ao criar a pasta Components, o namespace dela ficou afya_admin.Components.

Dificuldades e soluções
1. Erro de compilação por código no arquivo errado. Ao compilar, apareceram 4 erros (CS1001, CS1003, CS1002 e CS1022) apontando para o início do DashboardData.cs. O motivo é que eu tinha colado nele o conteúdo do Dashboard.razor (que começa com @page "/"), e o compilador C# não entende isso. Resolvi apagando o conteúdo do arquivo e colando o código correto dos dados, começando com using MudBlazor;, e depois o build passou sem erros. Aprendi a conferir em qual arquivo estou colando cada trecho.

2. Trabalho enviado para um branch, mas o main do GitHub ficou sem o código. Eu fiz commit e push em um branch de feature, mas a página principal do repositório (o main) só mostrava o commit inicial. Descobri que o GitHub mostra o main por padrão e que meu código estava no outro branch. Resolvi integrando o branch ao main (merge por Pull Request), e depois passei a trabalhar com um branch por etapa e a integrar cada um por Pull Request.

3. Sublinhado vermelho no editor mesmo com o código certo. No NavMenu.razor, o VS Code marcava erro nos MudChip, mas o código era igual ao do tutorial (com T="string"). Rodei dotnet build e ele terminou com 0 erros, então o aviso era só o editor desatualizado. Aprendi que o resultado do build é a referência, e que recarregar a janela do editor costuma limpar o aviso.

Melhorias futuras (opcional)
Fazer as outras páginas do menu. Hoje só o Dashboard existe. Os outros links, como Clientes e Projetos, levam para uma página de erro. criar essas páginas para o menu funcionar completo.
Fazer o botão de período mudar os números. Hoje, quando escolho "Últimos 7 dias", só o texto do botão muda. futuramente dados reais com os números dos cards também mudassem apresentado os dados corretos de cada periodo.
Fazer a busca funcionar. O campo "Pesquisar..." no topo ainda não faz nadafuturamente filtrar a tabela de Projetos Recentes.
Lembrar o tema escolhido. Quando recarrego a página, ela volta para o tema claro. proxima melhoria seria o navegador lembrar se escolhi o tema escuro ou claro.