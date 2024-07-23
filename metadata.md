---
solution: Experience Manager
type: Documentation
product: adobe experience manager
git-repo: https://github.com/AdobeDocs/experience-manager-htl.pt-BR
index: y
recommendations: noDisplay
source-git-commit: 22f62868df0fcfc558e5d62434dde843a9f3ca83
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

