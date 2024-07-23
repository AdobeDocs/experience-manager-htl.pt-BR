---
title: Histórico do HTL
description: Para usuários antigos do AEM, esse documento fornece informações sobre o HTL, sua mudança de nome (ele era anteriormente conhecido como Sightly) e como ele substitui o JSP.
exl-id: 00985b35-2130-4946-959a-0a09a34a0f05
source-git-commit: c6bb6f0954ada866cec574d480b6ea5ac0b51a3f
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 40%

---


# Histórico do HTL {#history-of-htl}

Para usuários antigos do AEM, esse documento fornece informações sobre o HTL, sua mudança de nome (ele era anteriormente conhecido como Sightly) e como ele substitui o JSP.

## Anteriormente conhecido como Sightly {#sightly}

O HTL (Linguagem de modelo HTML) é o sistema de modelo do lado do servidor preferencial e recomendado para HTML no Adobe Experience Manager. Substitui as páginas JSP (JavaServer Pages), conforme usadas em versões anteriores do AEM.

## HTL em vez de JSP {#htl-over-jsp}

O Adobe recomenda que, para novos projetos AEM, você use a Linguagem de modelo de HTML. O motivo é porque ele oferece vários benefícios em comparação ao JSP. No entanto, para projetos existentes, a migração só faz sentido se for estimado que o esforço será menor do que manter os JSPs existentes para os próximos anos.

Migrar para HTL não é necessariamente uma opção radical, pois os componentes gravados em HTL são compatíveis com componentes gravados em JSP ou ESP. Essa abordagem significa que os projetos existentes podem usar HTL para novos componentes sem problemas, mantendo o JSP para os componentes existentes.

Inclusive no mesmo componente, os arquivos HTL podem ser usados junto a JSPs e ESPs. O exemplo a seguir mostra em **linha 1** como incluir um arquivo HTL de um arquivo JSP, e em **linha 2** como um arquivo JSP pode ser incluído de um arquivo HTL:

```xml
<cq:include script="template.html"/>
<sly data-sly-include="template.jsp"/>
```

## Perguntas frequentes {#frequently-asked-questions}

Desenvolvedores experientes de AEM que são novos no HTL, geralmente fazem as seguintes perguntas:

### O HTL tem alguma limitação que o JSP não tem? {#limitations}

O HTL não tem limitações em comparação com o JSP. O que pode ser feito com o JSP também deve ser realizável com o HTL. No entanto, por design, o HTL é mais rigoroso que o JSP em vários aspectos. O que pode ser obtido em um único arquivo JSP pode precisar ser separado em uma classe Java ou em um arquivo JavaScript para ser realizável no HTL. Mas essa abordagem é geralmente desejada para garantir uma boa separação de interesses entre a lógica e a marcação.

### O HTL é compatível com Bibliotecas de tags JSP? {#tag-libraries}

Não. No entanto, conforme mostrado na seção [Carregamento de bibliotecas de clientes](getting-started.md#loading-client-libraries) do documento de introdução, as instruções de modelo e chamada oferecem um padrão semelhante.

### Os recursos do HTL podem ser estendidos em um projeto AEM? {#extended}

Não. O HTL tem mecanismos de extensão poderosos para reutilização de lógica (a [API de uso](#use-api-for-accessing-logic)) e de marcação (as declarações de modelo e chamada), que podem ser usados para modular o código dos projetos.

### Quais são os principais benefícios do HTL em relação ao JSP? {#benefits}

A segurança e a eficiência dos projetos são os principais benefícios, detalhados na [Visão geral](overview.md).

### O JavaServer Pages (JSP) está desaparecendo? {#go-away}

Não. Não há planos para descontinuar o JSP.

## Por que o nome foi alterado? {#what-is-in-a-name}

No AEM 6.0 e 6.1, o HTL foi chamado **Sightly**. O Adobe o renomeou para **Linguagem de Modelo do HTML** ou **HTL** para esclarecer a sua especificação e alinhar-se às diretrizes gerais de nomenclatura do Adobe. Essa alteração de nome entrou em vigor em agosto de 2016 e se aplica à versão AEM 6.0 e posteriores.

>[!NOTE]
>
>Essa alteração de nome não afeta o código ou a API, portanto, a compatibilidade não foi afetada. Para obter mais informações, [consulte este vídeo de comunicado](https://helpx.adobe.com/br/experience-manager/how-to/announce-htl.html).

Para saber mais sobre HTL, consulte o [Guia de Introdução à Linguagem de Modelo de HTML (HTL)](overview.md).
