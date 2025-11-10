---
solution: Experience Manager
type: Documentation
product: adobe experience manager
git-repo: https://github.com/AdobeDocs/experience-manager-htl.pt-BR
index: y
landing-page-name: experience-manager
landing-page-breadcrumb-title: AEM
recommendations: noDisplay
source-git-commit: 5c7a0f5795bcbb3b4a5fb34f2d49aad6aa31122f
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 40%

---


# Metadados para uso interno

O sistema de criação do GitHub define os metadados hierarquicamente, com níveis crescentes de precedentes, como visto a seguir:

1. metadata.md
1. Índice
1. Artigo

Os metadados definidos no arquivo metadata.md se aplicam a todo o repositório, mas podem ser substituídos nos níveis de índice e artigo. Qualquer substituição dos metadados deve ser feita no nível mais baixo possível.

Os metadados no repositório `experience-manager-core-components.en` são o mínimo necessário.

metadata.md

* `product`
* `git-repo`
* `index: y`

Não está mais em uso:

* `solution-title`
* `solution-hub-url`
* `getting-started-title`
* `getting-started-url`
* `tutorials-title`
* `tutorials-url`

Índices

* `sub-product`
* `user-guide-title`

Artigo

* `title`
* `description`
* `index: n` (somente para versões anteriores de componentes)

