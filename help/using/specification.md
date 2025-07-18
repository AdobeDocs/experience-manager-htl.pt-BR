---
title: A especificação do HTL
description: Consulte a Especificação do HTL para obter informações detalhadas sobre sintaxe.
exl-id: c0657476-4db6-4fad-ad87-9252b5003237
source-git-commit: addc69e4b4e56a9b1c5f91ce9af26fa2d326d981
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 62%

---


# A especificação do HTL {#htl-specification}

A Linguagem de modelo HTML (HTL) é o sistema de modelo do lado do servidor preferencial e recomendado para HTML.

## Camadas HTL {#layers}

É possível definir a HTL no AEM por várias camadas.

1. **[Especificação do HTL](https://github.com/adobe/htl-spec)** - o HTL é uma especificação de código aberto e independente de plataforma que pode ser implementada livremente por qualquer pessoa. Suas especificações são mantidas no repositório do GitHub.
1. **[Mecanismo de script HTL do Sling](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html)** - O projeto `Sling` criou a implementação de referência do HTL, que é usada pelo AEM. O projeto `Sling` mantém sua documentação.
1. **[Extensões do AEM](aem-extensions.md)** - O AEM se baseia no Mecanismo de Script HTL `Sling` para oferecer aos desenvolvedores recursos convenientes e específicos ao AEM. Essas extensões são documentadas como parte desse conjunto de documentações.

Utilize os links acima para acessar a documentação dedicada de todas as camadas do HTL usadas pelo AEM.
