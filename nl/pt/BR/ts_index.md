---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolução de problemas
{: #troubleshooting}

Se você tiver problemas enquanto está trabalhando com o {{site.data.keyword.openwhisk}}, considere estas técnicas para resolução de problemas e obtenção de ajuda.
{: shortdesc}



## A ação está falhando
{: #ts_action_fails}

{: tsSymptoms}
A ação está falhando.

{: tsCauses}
Uma explicação para um app com falha é que um tempo de execução descontinuado está sendo usado. Uma ação não pode ser concluída com êxito até que o tempo de execução seja atualizado para um suportado.

{: tsResolve}
Para ver qual tempo de execução é usado para a ação, execute `ibmcloud fn action get ACTION_NAME` e verifique `deprecated=true` na resposta da consulta. Se o tempo de execução foi descontinuado, [atualize o tempo de execução](/docs/openwhisk?topic=cloud-functions-actions#actions_update).


<br />


## Limites do sistema foram atingidos
{: #ts_limit_reached}

{: tsSymptoms}
Sua função não está sendo executada corretamente e pode resultar em uma mensagem como `{"error":"signal: killed"}`.

{: tsCauses}
Todas as funções têm [limites do sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) mínimos e máximos, como o máximo de uso de memória ou um tempo limite.

{: tsResolve}
Alguns limites poderão ser aumentados se um business case puder justificar valores de limite de segurança mais altos. Para aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do console da web do IBM Cloud Functions.

1. Selecione **Suporte**.
2. Selecione **Incluir chamado** no menu suspenso.
3. Selecione **Técnico** para o tipo de chamado.
4. Selecione **Functions** para a Área de suporte técnico.


<br />


## Obtendo ajuda e suporte
{: #gettinghelp}

Ainda tendo problemas com a sua função?
{: shortdesc}

-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status).
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
    -   Se você tiver perguntas técnicas sobre o desenvolvimento de funções com o {{site.data.keyword.openwhisk}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://stackoverflow.com/search?q=ibm-cloud-functions) e identifique-a com `ibm-cloud-functions`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum do [IBM Developer Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `functions`. Consulte
[Obtendo
ajuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obter mais detalhes sobre o uso dos fóruns.
-   Entre em contato com o Suporte IBM abrindo um caso. Para saber mais sobre como abrir um caso de suporte IBM ou sobre os níveis de suporte e as severidades do caso, consulte [Entrando em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support).
Quando você relatar um problema, inclua seu ID de ativação. Para obter um ID de ativação, execute `ibmcloud fn activation list`.
{: tip}
