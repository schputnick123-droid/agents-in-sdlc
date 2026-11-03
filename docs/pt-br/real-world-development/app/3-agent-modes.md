---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para se integrar ao fluxo de trabalho existente da organização e ajudar a construir as soluções certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes, começando com uma issue para gerar um novo recurso, confirmando se o código é válido e se o recurso se comporta como esperado e, por fim, fazendo o merge no projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de desenvolvimento do recurso. Em geral, você teria sessões ou PRs diferentes para cada tipo de arquivo, mas neste workshop vamos simplificar o processo para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente a partir de uma issue do GitHub;
- definir requisitos no modo **Plan**;
- implementar o recurso no modo **Autopilot**;
- revisar o código;
- validar manualmente o recurso em um canvas de navegador.

Enquanto desenvolve esse recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação por MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas alguns detalhes — como a combinação de categorias — precisam ser definidos antes de começar a programação. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles ficam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo semelhante a este:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Criar um plano do que precisa ser implementado.
3. Implementar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue uma variação desse processo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão usado para código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive:** você trabalha junto com o agente. Ele sugere alterações e aguarda sua orientação antes de continuar.
- **Plan:** o agente cria primeiro um plano. Você o revisa e aprova antes de permitir que o agente o execute.
- **Autopilot:** o agente trabalha de forma totalmente autônoma: escreve código, executa testes e itera sem aguardar sua orientação.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme se o PR das avaliações por estrelas foi mesclado e se sua branch `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, em seguida, **Issues** na navegação superior do repositório. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada a partir do modelo; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH ou a configuração do gerenciador de versões do aplicativo e feche e reabra o aplicativo completamente. Tente novamente a operação e as verificações com falha; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva o código. Como você começou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Verifique se ele abrange a filtragem por categoria e editora, controles acessíveis, alterações no acesso a dados e testes. Discuta qualquer comportamento que não esteja claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes de unidade, testes E2E e verificação de tipos, usando as ferramentas existentes do projeto. Mantenha-o focado na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Solicite alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e quaisquer esclarecimentos acordados para a validação posterior.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme o modo ativo em vez de presumir que a aprovação o altera. Verifique se o indicador mostra **Autopilot**.

O Copilot começará a trabalhar na implementação. Você verá que ele seguirá o plano de forma iterativa, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa apenas os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo está correto.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de várias categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes de unidade, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use o seguinte prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro e, em seguida, selecione uma editora e confirme os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se várias categorias forem aceitas, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que interrompa o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, a worktree e a branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao QA antes de criar e mesclar o PR combinado na Lição 8. Depois de interromper o servidor de desenvolvimento, você pode deixar a sessão ociosa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova sessão de agente a partir de uma issue do GitHub;
- definiu requisitos no modo **Plan**;
- implementou o recurso no modo **Autopilot**;
- revisou o código;
- validou manualmente o recurso em um canvas de navegador.

Em seguida, você conhecerá melhor a geração de código e aprenderá a garantir que ele siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
