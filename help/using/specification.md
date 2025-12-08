---
title: A especificação do HTL
description: Consulte a especificação da HTL para informações detalhadas da sintaxe.
exl-id: c0657476-4db6-4fad-ad87-9252b5003237
index: no
TQID: https://experienceleague.adobe.com/2SNWq2C9a1awkjaGhBmx7I3LgwcyhuSdcG26rodtHqU
product_v2: id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a9c0f2ea176e8226d8f3eb30ecff63ebafd3e2ae
workflow-type: tm+mt
source-wordcount: 134
ht-degree: 100%

---

# A especificação do HTL {#htl-specification}

A Linguagem de modelo HTML (HTL) é o sistema de modelo do lado do servidor preferencial e recomendado para HTML.

## Camadas HTL {#layers}

É possível definir a HTL no AEM por várias camadas.

1. **[Especificação do HTL](https://github.com/adobe/htl-spec)** - o HTL é uma especificação de código aberto e independente de plataforma que pode ser implementada livremente por qualquer pessoa. Suas especificações são mantidas no repositório do GitHub.
1. **[Mecanismo de script HTL do Sling](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html)**: o projeto do `Sling` criou a implementação de referência da HTL usada pelo AEM. O projeto do `Sling` mantém sua documentação.
1. **[Extensões do AEM](aem-extensions.md)**: o AEM baseia-se no mecanismo de script HTL do `Sling` para oferecer a desenvolvedores recursos convenientes específicos do AEM. Essas extensões são documentadas como parte desse conjunto de documentações.

Utilize os links acima para acessar a documentação dedicada de todas as camadas do HTL usadas pelo AEM.
