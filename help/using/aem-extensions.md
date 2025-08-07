---
title: Extensões do AEM
description: O AEM oferece extensões de especificação HTL para AEM a fim de proporcionar comodidade a desenvolvedores.
exl-id: d78cb84d-f958-45e2-9c6c-df86a68277d5
index: false
source-git-commit: a496d23277902a5cd573a6a8af770f27b0269f05
workflow-type: ht
source-wordcount: '228'
ht-degree: 100%

---


# Extensões do AEM {#aem-extensions}

Semelhante às [Extensões da especificação da HTL para o Apache Sling](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#extensions-of-the-htl-specification-1), o AEM oferece algumas opções de expressão adicionais que tornam mais fácil trabalhar com conceitos do AEM diretamente nos scripts HTL.

## i18n {#i18n}

As mesmas [três opções adicionais](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#i18n) do Apache Sling podem ser usadas junto com o `i18n`:

* `locale`
* `hint`
* `basename`

No entanto, no AEM, o [suporte à internacionalização](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/developing/components/internationalization/i18n-dev) para HTL é implementado com a ajuda da API do pacote `com.day.cq.i18n`.

## `data-sly-include` {#data-sly-include}

No AEM, `data-sly-include` pode receber uma opção `wcmmode` adicional que controla o [Modo WCM](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/WCMMode.html) do script incluído. Os valores permitidos são os nomes das constantes de enumeração disponíveis.

## `data-sly-resource` {#data-sly-resource}

Além dos caminhos e `Resources`, o elemento de bloco `data-sly-resource` também pode funcionar com [`Maps`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html) ou [`Records`](https://github.com/apache/sling-org-apache-sling-scripting-sightly-runtime/blob/master/src/main/java/org/apache/sling/scripting/sightly/Record.java). Em ambas as abordagens, a propriedade de string `resourceName` deve ser fornecida. Seu valor é usado para criar um [recurso sintético](https://www.javadoc.io/doc/org.apache.sling/org.apache.sling.api/latest/org/apache/sling/api/resource/SyntheticResource.html) que é incluído no contexto de renderização. O restante das propriedades do `Record` ou `Map` transmitidas para o `data-sly-resource` será usado como propriedades de `Resource` normais. Se a propriedade `sling:resourceType` estiver ausente nesse mapa, o tipo de recurso será considerado como o valor da [opção de expressão](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#229-resource) `resourceType` ou o tipo do recurso atual que orienta a renderização.

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
