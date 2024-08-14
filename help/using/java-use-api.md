---
title: API de uso Java do HTL
description: A API de uso Java do HTL permite que um arquivo HTL acesse métodos de ajuda em uma classe Java personalizada.
exl-id: 9a9a2bf8-d178-4460-a3ec-cbefcfc09959
source-git-commit: 5e1dce693dc61300530837c996f45d793c0b07e6
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 100%

---


# API de uso Java do HTL {#htl-java-use-api}

A API de uso Java do HTL permite que um arquivo HTL acesse métodos de ajuda em uma classe Java personalizada.

## Caso de uso {#use-case}

A API de uso Java do HTL permite que um arquivo HTL acesse métodos de ajuda em uma classe Java personalizada através do `data-sly-use`. Esse método permite que toda lógica de negócios complexa seja encapsulada no código Java, enquanto o código HTL lida somente com a produção de marcação direta.

Um objeto da API de uso do Java pode ser um POJO simples, instanciado por uma implementação específica por meio do construtor padrão do POJO.

Os POJOs da API de uso também podem expor um método público, chamado init, com a seguinte assinatura:

```java
    /**
     * Initializes the Use bean.
     *
     * @param bindings All bindings available to the HTL scripts.
     **/
    public void init(javax.script.Bindings bindings);
```

O mapa de `bindings` pode conter objetos que fornecem contexto para o script HTL executado no momento que o objeto da API de uso pode usar para processamento.

## Um exemplo simples {#a-simple-example}

Este exemplo ilustra a utilização da API de uso.

>[!NOTE]
>
>Este exemplo foi simplificado apenas para ilustrar o uso. Em um ambiente de produção, a Adobe recomenda usar [modelos do Sling](https://sling.apache.org/documentation/bundles/models.html).

Comece com um componente HTL, chamado de `info,`, que não tenha nenhuma classe de uso. Ele consiste em um único arquivo, `/apps/my-example/components/info.html`

```xml
<div>
    <h1>${properties.title}</h1>
    <p>${properties.description}</p>
</div>
```

Adicione algum conteúdo para esse componente renderizar em `/content/my-example/`:

```xml
{
    "sling:resourceType": "my-example/component/info",
    "title": "My Example",
    "description": "This Is Some Example Content."
}
```

Ao acessar esse conteúdo, o arquivo HTL será executado. No código HTL, o objeto de contexto `properties` é usado para acessar e exibir o `title` e o `description` do recurso atual. O arquivo de saída `/content/my-example.html` é o seguinte:

```html
<div>
    <h1>My Example</h1>
    <p>This Is Some Example Content.</p>
</div>
```

### Adicionar uma classe de uso {#adding-a-use-class}

O componente `info` na sua versão atual não precisa de uma classe de uso para executar sua função simples. Entretanto, há casos em que é necessário fazer coisas que não podem ser feitas no HTL e, portanto, você precisa de uma classe de uso. Mas lembre-se:

>[!NOTE]
>
>Uma classe de uso só deve ser usada quando algo não pode ser feito somente em HTL.

Por exemplo, suponha que você deseja que o componente `info` exiba as propriedades `title` e `description` do recurso, mas todas em minúsculas. Como a HTL não tem um método para deixar as strings em letras minúsculas, é necessário adicionar uma classe de uso Java e alterar `/apps/my-example/component/info/info.html` da seguinte maneira:

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

Além disso, `/apps/my-example/component/info/Info.java` é criado.

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {
    private String lowerCaseTitle;
    private String lowerCaseDescription;

    @Override
    public void activate() throws Exception {
        lowerCaseTitle = getProperties().get("title", "").toLowerCase();
        lowerCaseDescription = getProperties().get("description", "").toLowerCase();
    }

    public String getLowerCaseTitle() {
        return lowerCaseTitle;
    }

    public String getLowerCaseDescription() {
        return lowerCaseDescription;
    }
}
```

Consulte a [documentação do Java sobre `com.adobe.cq.sightly.WCMUsePojo`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html) para obter mais detalhes.

Agora, vamos analisar as diferentes partes do código.

### Classe de Java local vs. pacote {#local-vs-bundle-java-class}

A classe de uso Java pode ser instalada de duas maneiras:

* **Local** - em uma instalação local, o arquivo de origem Java é colocado junto do arquivo HTL, na mesma pasta do repositório. A origem é automaticamente compilada sob demanda. Não é necessária nenhuma etapa de compilação ou empacotamento separada.
* **Pacote** - em uma instalação de pacote, a classe Java deve ser compilada e implantada em um pacote OSGi usando o mecanismo padrão de implantação de pacote do AEM (consulte a seção [Classe Java agrupada](#bundled-java-class)).

Para saber quando usar cada método, lembre-se destes dois pontos:

* Uma **classe de uso local do Java** é recomendada quando a classe de uso é específica para o componente em questão.
* Uma **classe de uso em pacote do Java** é recomendada quando o código Java implementa um serviço acessado de vários componentes HTL.

Este exemplo usa uma instalação local.

### O pacote Java é o caminho do repositório {#java-package-is-repository-path}

Ao usar uma instalação local, o nome do pacote da classe de uso precisa corresponder ao local da pasta de repositório. Os sublinhados no nome do pacote substituem os hifens no caminho.

Neste caso, `Info.java` está localizado em `/apps/my-example/components/info`, portanto, o pacote é `apps.my_example.components.info`:

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {

   ...

}
```

>[!NOTE]
>
>Usar hifens nos nomes dos itens do repositório é uma prática recomendada no desenvolvimento do AEM. No entanto, os hifens são inválidos nos nomes dos pacotes Java. Por esse motivo, **todos os hifens no caminho do repositório devem ser convertidos em sublinhados no nome do pacote**.

### Extensão `WCMUsePojo` {#extending-wcmusepojo}

Embora existam várias maneiras de incorporar uma classe de Java com HTL, o mais simples é estender a classe `WCMUsePojo`. Para este exemplo de `/apps/my-example/component/info/Info.java`:

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo

    ...
}
```

### Inicializar a classe {#initializing-the-class}

Quando a classe de uso é estendida de `WCMUsePojo`, a inicialização é executada substituindo o método `activate`; nesse caso, em `/apps/my-example/component/info/Info.java`

```java
...

public class Info extends WCMUsePojo {
    private String lowerCaseTitle;
    private String lowerCaseDescription;

    @Override
    public void activate() throws Exception {
        lowerCaseTitle = getProperties().get("title", "").toLowerCase();
        lowerCaseDescription = getProperties().get("description", "").toLowerCase();
    }

...

}
```

### Contexto {#context}

Normalmente, o método [ativar](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html) é usado para pré-calcular e armazenar (em variáveis de membro) os valores necessários no código HTL, com base no contexto atual (na solicitação e no recurso atuais, por exemplo).

A classe `WCMUsePojo` fornece acesso ao mesmo conjunto de objetos de contexto que estão disponíveis em um arquivo HTL (consulte o documento [Objetos globais](global-objects.md)).

Em um `WCMUsePojo` de extensão de classe, é possível acessar objetos de contexto pelo nome:

[`<T> T get(String name, Class<T> type)`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html)

Alternativamente, é possível acessar diretamente os objetos de contexto comumente usados por meio do método apropriado contido nesta tabela.

| Objeto | Método de conveniência |
|---|---|
| [`PageManager`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/PageManager.html) | [`getPageManager()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getPageManager--) |
| [`Page`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/Page.html) | [`getCurrentPage()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentPage--) |
| [`Page`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/Page.html) | [`getResourcePage()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResourcePage--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getPageProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getPageProperties--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getProperties--) |
| [`Designer`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Designer.html) | [`getDesigner()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getDesigner--) |
| [`Design`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Design.html) | [`getCurrentDesign()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentDesign--) |
| [`Style`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Style.html) | [`getCurrentStyle()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentStyle--) |
| [`Component`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/components/Component.html) | [`getComponent()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getComponent--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getInheritedProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getInheritedPageProperties--) |
| [`Resource`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/Resource.html) | [`getResource()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResource--) |
| [`ResourceResolver`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ResourceResolver.html) | [`getResourceResolver()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResourceResolver--) |
| [`SlingHttpServletRequest`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/SlingHttpServletRequest.html) | [`getRequest()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getRequest--) |
| [`SlingHttpServletResponse`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/SlingHttpServletResponse.html) | [`getResponse()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResponse--) |
| [`SlingScriptHelper`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/scripting/SlingScriptHelper.html) | [`getSlingScriptHelper()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getSlingScriptHelper--) |

### Métodos Getter {#getter-methods}

Depois que a classe de uso for inicializada, o arquivo HTL será executado. Durante esse estágio, o HTL geralmente obtém o estado de diversas variáveis de membros da classe de uso e as renderiza para fins de apresentação.

Para fornecer acesso a esses valores a partir do arquivo HTL, defina métodos Getter personalizados na classe de uso de acordo com a seguinte convenção de nomeação:

* Um método do formulário `getXyz` expõe no arquivo HTL uma propriedade de objeto chamada `xyz`.

No arquivo de exemplo a seguir, `/apps/my-example/component/info/Info.java`, os métodos `getTitle` e `getDescription` tornam as propriedades de objeto `title` e `description` acessíveis no contexto do arquivo HTL.

```java
...

public class Info extends WCMUsePojo {

    ...

    public String getLowerCaseTitle() {
        return lowerCaseTitle;
    }

    public String getLowerCaseDescription() {
        return lowerCaseDescription;
    }
}
```

### Atributo `data-sly-use` {#data-sly-use-attribute}

O atributo `data-sly-use` é usado para inicializar a classe de uso no código HTL. No exemplo, o atributo `data-sly-use` declara que a classe `Info` é usada. É possível usar apenas o nome local da classe, porque estamos usando uma instalação local (uma vez que o arquivo de origem Java tenha sido colocado na mesma pasta que o arquivo HTL). Se estivesse usando uma instalação de pacote, seria necessário especificar o nome de classe totalmente qualificado.

Observe o uso neste exemplo de `/apps/my-example/component/info/info.html`.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Identificador local {#local-identifier}

O identificador `info` (após o ponto em `data-sly-use.info`) é usado no arquivo HTL para identificar a classe. O escopo desse identificador é global dentro do arquivo, depois que ele tiver sido declarado. Não está limitado ao elemento que contém a instrução `data-sly-use`.

Observe o uso neste exemplo de `/apps/my-example/component/info/info.html`.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Obter propriedades {#getting-properties}

O identificador `info` é então usado para acessar as propriedades do objeto `title` e `description`, que foram expostas por meio dos métodos Getter `Info.getTitle` e `Info.getDescription`.

Observe o uso neste exemplo de `/apps/my-example/component/info/info.html`.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Saída {#output}

Agora, ao acessar `/content/my-example.html`, ele retorna o seguinte arquivo `/content/my-example.html`.

```xml
<div>
    <h1>my example</h1>
    <p>this is some example content.</p>
</div>
```

>[!NOTE]
>
>Esse exemplo foi simplificado apenas para ilustrar seu uso. Em um ambiente de produção, a Adobe recomenda usar [modelos Sling](https://sling.apache.org/documentation/bundles/models.html).

## Além das noções básicas {#beyond-the-basics}

Esta seção apresenta alguns recursos adicionais que vão além do exemplo simples descrito anteriormente.

* Enviar parâmetros para uma classe de uso
* Classe de uso Java agrupada

### Passagem de parâmetros {#passing-parameters}

Os parâmetros podem ser passados para uma classe de uso mediante inicialização.

Para obter detalhes, consulte a [documentação do Mecanismo de script HTL](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#passing-parameters-to-java-use-objects) do Sling.

### Classe Java agrupada {#bundled-java-class}

Com uma classe de uso agrupada, a classe deve ser compilada, agrupada e implantada no AEM usando o mecanismo de implantação de pacote OSGi padrão. Diferente de uma instalação local, a declaração do pacote de classe de uso deve ser nomeada normalmente, como neste exemplo de `/apps/my-example/component/info/Info.java`.

```java
package org.example.app.components;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {
    ...
}
```

E a declaração `data-sly-use` deve fazer referência a um nome de classe totalmente qualificado, em vez de apenas ao nome de classe local, como neste exemplo de `/apps/my-example/component/info/info.html`.

```xml
<div data-sly-use.info="org.example.app.components.info.Info">
  <h1>${info.title}</h1>
  <p>${info.description}</p>
</div>
```
