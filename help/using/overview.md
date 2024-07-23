---
title: Visão geral do HTL
description: Descubra como o AEM suporta HTL (HTML Template Language) para fornecer uma estrutura Web produtiva de nível empresarial que melhore a segurança. Essa estrutura permite que desenvolvedores de HTML sem conhecimento em Java participem melhor de projetos AEM.
exl-id: 5d06ff25-d681-4b95-8375-c28a8364eb7e
source-git-commit: c6bb6f0954ada866cec574d480b6ea5ac0b51a3f
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 26%

---


# Visão geral {#overview}

A Linguagem de modelo de HTML (HTL), compatível com o Adobe Experience Manager (AEM), tem como objetivo fornecer uma estrutura corporativa da Web altamente produtiva que aumenta a segurança. Ela também permite que desenvolvedores de HTML sem conhecimento em Java participem melhor de projetos AEM.

[Introduzida no AEM 6.0](history.md), a Linguagem de Modelo de HTML é o sistema de modelo preferencial e recomendado do lado do servidor para o HTML AEM no. Para desenvolvedores da Web que precisam criar sites corporativos robustos, a Linguagem de modelo HTML ajuda a aumentar a segurança e a eficiência do desenvolvimento.

## Maior segurança {#increased-security}

A Linguagem de modelo de HTML (HTL) aumenta a segurança do site aplicando automaticamente o escape com reconhecimento de contexto a todas as variáveis de saída, tornando-o mais seguro do que a maioria dos outros sistemas de modelo. O HTL torna essa abordagem possível porque entende a sintaxe de HTML e usa esse conhecimento para ajustar o escape necessário para expressões, com base em sua posição na marcação. Este método pode resultar em expressões colocadas em atributos `href` ou `src` para serem evitadas de forma diferente das expressões colocadas em outros atributos ou locais.

Embora o mesmo resultado possa ser obtido com linguagens de modelo como o JSP, o desenvolvedor deve garantir manualmente que o escape adequado seja aplicado a cada variável. Como uma única omissão ou erro no escape aplicado possivelmente é suficiente para causar uma vulnerabilidade na criação de script entre sites (XSS), o Adobe decidiu automatizar essa tarefa com HTL. Se necessário, os desenvolvedores ainda podem especificar um escape diferente nas expressões, mas com HTL, o comportamento padrão está muito mais propenso a corresponder ao comportamento desejado, reduzindo a probabilidade de erros.

## Desenvolvimento simplificado {#simplified-development}

A Linguagem de modelo HTML é fácil de aprender e seus recursos são limitados de propósito para garantir que permaneça simples e direta. Ela também tem mecanismos poderosos para estruturar a marcação e chamar a lógica, ao mesmo tempo que impõe sempre a separação rigorosa das preocupações entre marcação e lógica. HTL é o HTML5 padrão, que usa expressões e atributos de dados para anotar a marcação com comportamento dinâmico. Essa abordagem mantém a validade e a legibilidade da marcação. A avaliação das expressões e dos atributos de dados é feita totalmente no lado do servidor e não é visível no lado do cliente, onde qualquer estrutura do JavaScript desejada pode ser usada sem interferência.

Esses recursos permitem que desenvolvedores de HTML sem conhecimento em Java editem modelos HTL, integrem-se à equipe de desenvolvimento e simplifiquem a colaboração com desenvolvedores Java de pilha completa. E vice-versa, ela permite que os desenvolvedores Java se concentrem no código back-end sem se preocuparem com o HTML.

## Menor custo {#reduced-costs}

Para projetos AEM, maior segurança, desenvolvimento simplificado e melhor colaboração em equipe resultam em menos esforço, tempo de entrada no mercado (TTM) mais rápido e menor custo total de propriedade (TCO).

A reimplementação do site Adobe.com com a Linguagem de modelo de HTML mostrou que os custos e a duração do projeto são reduzidos em até aproximadamente 25%.

![Aumente a eficiência e diminua os custos](assets/chlimage_1.png)

O diagrama acima mostra que possivelmente as seguintes melhorias na eficiência foram viabilizadas pelo HTL:

* **Os desenvolvedores de HTML / CSS / JS:** HTML podem editar diretamente os modelos HTL, permitindo que os designs de front-end sejam implementados diretamente nos componentes do AEM, eliminando a necessidade de implementação separada. Essa abordagem reduz iterações negativas com desenvolvedores Java de pilha completa.
* **JSP / HTL:** como o HTL em si não requer conhecimento em Java e tem gravação direta, qualquer desenvolvedor com conhecimento em HTML pode editar os modelos.
* **Java:** graças à API de uso clara e simples de usar fornecida pelo HTL, a interface com a lógica comercial é esclarecida, o que também beneficia o desenvolvimento do Java em geral.

## Vídeo de introdução {#video}

O vídeo a seguir, retirado de uma [sessão de Gems do AEM](https://experienceleague.adobe.com/en/docs/events/experience-manager-gems-recordings/gems2014/aem-introduction-to-htl), fornece uma visão geral da finalidade do HTL, bem como exemplos de implementação.

>[!VIDEO](https://video.tv.adobe.com/v/19504/?quality=9)

Observe que o vídeo se refere ao HTL por [seu antigo nome, Sightly](history.md).

## Próximas etapas {#next-steps}

Agora que você conhece os objetivos e as vantagens do HTL, pode começar com a linguagem. Consulte [Introdução à Linguagem de Modelo do HTML](getting-started.md).
