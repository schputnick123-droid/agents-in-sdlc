---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) das avaliações por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, todo o fluxo de filtragem e qualidade foi mantido em uma sessão e uma branch, e o trabalho será incluído neste PR.

Nesta lição, você vai:

- conhecer o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionar o PR completo do recurso e as evidências de verificação;
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o merge — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e faz o merge assim que o GitHub permite. Ele trabalha em segundo plano, continua funcionando após reinicializações do aplicativo e é desativado automaticamente quando o pull request é mesclado.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode cuidar dessa etapa, mas ainda precisa de autorização explícita para editar código e fazer merge. Revise as ações permitidas e o trabalho antes de conceder permissão de merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para escolher **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Apenas escolher a opção no menu suspenso não inicia o processo.

O processo do Agent Merge começa. Ele vai:

- criar o pull request com título e descrição;
- incluir uma referência à issue relacionada no corpo da descrição se você tiver iniciado a sessão a partir de uma issue;
- fazer rebase ou lidar com possíveis conflitos de merge com a branch de destino;
- acompanhar o processo de CI para garantir que todas as verificações sejam aprovadas;
- acompanhar comentários de outras pessoas desenvolvedoras e da revisão de código do Copilot e atualizar o PR para resolvê-los;
- opcionalmente, fazer o merge automático do PR quando tudo for concluído com sucesso.

Vamos permitir que o Agent Merge também faça o merge do PR quando tudo for aprovado.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão de merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Confirme se o status do PR é **Merged** antes de continuar; habilitar a permissão de merge ou ver um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, teste e validação de código e, agora, o processo do pull request. Você:

- conheceu o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionou o PR completo do recurso e as evidências de verificação;
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
