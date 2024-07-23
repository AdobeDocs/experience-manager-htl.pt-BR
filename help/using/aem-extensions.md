---
title: Extensões do AEM
description: A AEM oferece extensões da especificação do HTL para AEM para sua conveniência como desenvolvedor.
exl-id: d78cb84d-f958-45e2-9c6c-df86a68277d5
source-git-commit: ebeac25c38b81c92011c163c7860688f43547a7d
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 64%

---

# Extensões do AEM {#aem-extensions}

Semelhante às [Extensões da especificação do HTL para o Apache Sling](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#extensions-of-the-htl-specification-1), o AEM oferece algumas opções de expressão adicionais que tornam mais fácil trabalhar com conceitos do AEM diretamente nos scripts HTL.

## i18n {#i18n}

As mesmas [três opções adicionais](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#i18n) do Apache Sling podem ser usadas junto com o `i18n`:

* `locale`
* `hint`
* `basename`

No entanto, no AEM, o [suporte à internacionalização](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/components/internationalization/i18n-dev) para HTL é implementado com a ajuda da API do pacote `com.day.cq.i18n`.

## `data-sly-include` {#data-sly-include}

No AEM, `data-sly-include` pode receber uma opção `wcmmode` adicional que controla o [Modo WCM](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/WCMMode.html) do script incluído. Os valores permitidos são os nomes das constantes de enumeração disponíveis.

## `data-sly-resource` {#data-sly-resource}

Além dos caminhos e `Resources`, o elemento de bloco `data-sly-resource` também pode funcionar com [`Maps`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html) ou [`Records`.](https://github.com/apache/sling-org-apache-sling-scripting-sightly-runtime/blob/master/src/main/java/org/apache/sling/scripting/sightly/Record.java) Em ambas as abordagens, a propriedade de string `resourceName` deve ser fornecida. Seu valor é usado para criar um [Recurso Sintético](https://www.javadoc.io/doc/org.apache.sling/org.apache.sling.api/latest/org/apache/sling/api/resource/SyntheticResource.html) incluído no contexto de renderização. O restante das propriedades de `Record` ou `Map` passadas para `data-sly-resource` são usadas como propriedades `Resource` normais. Se a propriedade `sling:resourceType` estiver ausente neste mapa, o tipo de recurso será o valor da `resourceType` [opção de expressão](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#229-resource) ou o tipo do recurso atual que orienta a renderização.

Considerando as seguintes propriedades de mapa/registro disponíveis no escopo do script como `map`:

```javascript
{
    resourceName: "myText",
    "sling:resourceType": "core/wcm/components/text/v2/text",
    "text": "Hello World!"
}
```

E considerando a seguinte marcação:

```html
<div class="outer" data-sly-resource="${map}"></div>
```

A seguinte saída é esperada:

```html
<div class="outer">
    <div class="myText">
        <div data-cmp-data-layer="{&quot;text-e58d65c472&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;xdm:text&quot;:&quot;<p>Hello world!</p>&quot;}}" id="text-e58d65c472" class="cmp-text">
            <p>Hello world!</p>
        </div>
  </div>
</div>
```
