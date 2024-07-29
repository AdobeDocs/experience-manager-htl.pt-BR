---
title: Introdução ao HTL
description: Saiba mais sobre HTL, o sistema de modelo do lado do servidor preferencial e recomendado para HTML no AEM, e entenda os principais conceitos dessa linguagem e seus elementos fundamentais.
exl-id: c95eb1b3-3b96-4727-8f4f-d54e7136a8f9
source-git-commit: c6bb6f0954ada866cec574d480b6ea5ac0b51a3f
workflow-type: tm+mt
source-wordcount: '2050'
ht-degree: 99%

---


# Introdução ao HTL {#getting-started-with-htl}

O HTL (Linguagem de modelo HTML) é o sistema de modelo do lado do servidor preferencial e recomendado para HTML no Adobe Experience Manager. Como em todos os sistemas de modelos HTML do lado do servidor, um arquivo HTL define a saída enviada para o navegador especificando o próprio HTML, alguma lógica de apresentação básica e as variáveis a serem avaliadas no tempo de execução.

Este documento fornece uma visão geral da finalidade do HTL, bem como uma introdução aos conceitos e elementos fundamentais da linguagem.

>[!TIP]
>
>Este documento apresenta a finalidade do HTL e uma visão geral de sua estrutura e conceitos fundamentais. Em caso de dúvidas sobre sintaxes específicas, consulte a [Especificação do HTL](specification.md).

## Camadas HTL {#layers}

O AEM utiliza várias camadas para definir o HTL.

1. **[Especificação do HTL](specification.md)** - o HTL é uma especificação de código aberto e independente de plataforma que pode ser implementada livremente por qualquer pessoa.
1. **[Mecanismo de script HTL do Sling](specification.md)** - o projeto Sling criou a implementação de referência do HTL, que é usada pelo AEM.
1. **[Extensões do AEM](specification.md)**: o AEM baseia-se no mecanismo de script HTL do Sling para oferecer a desenvolvedores recursos convenientes específicos do AEM.

Esta documentação se limita ao uso do HTL para desenvolvimento de soluções no AEM. Assim, ela abrange as três camadas, fornecendo links para recursos externos conforme necessário.

## Conceitos fundamentais do HTL {#fundamental-concepts-of-htl}

A Linguagem de modelo HTML usa uma linguagem de expressão para inserir partes de conteúdo na marcação renderizada, e atributos de dados HTML5 para definir instruções sobre blocos de marcação (como condições ou iterações). À medida que o HTL é compilado em Servlets Java, as expressões e os atributos de dados HTL são avaliados totalmente no lado do servidor e nada permanece visível no HTML resultante.

>[!TIP]
>
>Para executar a maioria dos exemplos fornecidos nesta página, um ambiente de execução ativo chamado [Read Eval Print Loop](https://github.com/adobe/aem-htl-repl) pode ser usado.

### Blocos e expressões {#blocks-and-expressions}

Este é um primeiro exemplo, que pode estar contido como está em um arquivo `template.html`:

```xml
<h1 data-sly-test="${properties.jcr:title}">
    ${properties.jcr:title}
</h1>
```

Dois tipos diferentes de sintaxe podem ser identificados:

* **Declarações em bloco**: se quiser exibir o elemento `<h1>` condicionalmente, use um atributo de dados HTML5 `data-sly-test`. O HTL fornece vários atributos como esse, que permitem anexar um comportamento a qualquer elemento HTML, e todos recebem o prefixo `data-sly`.
* **Linguagem de expressão**: os caracteres `${` e `}` delimitam expressões HTL. No tempo de execução, essas expressões são avaliadas e seu valor é inserido no fluxo HTML de saída.

Consulte a [Especificação do HTL](specification.md) para obter detalhes sobre ambas as sintaxes.

### O elemento SLY {#the-sly-element}

Um conceito central do HTL é oferecer a possibilidade de reutilizar elementos HTML existentes para definir declarações em bloco. Essa reutilização evita a necessidade de inserir delimitadores adicionais para definir onde a declaração começa e termina. A anotação da marcação transforma de maneira não invasiva o HTML estático em um modelo dinâmico sem corromper a validade do HTML, garantindo a exibição adequada, mesmo na forma de arquivos estáticos.

No entanto, às vezes, pode não haver um elemento existente no local exato em que uma instrução em bloco deve ser inserida. Nesses casos, é possível inserir um elemento `sly` especial. Esse elemento é removido automaticamente da saída ao executar as declarações em bloco anexadas e exibir seu conteúdo adequadamente.

O exemplo a seguir:

```xml
<sly data-sly-test="${properties.jcr:title && properties.jcr:description}">
    <h1>${properties.jcr:title}</h1>
    <p>${properties.jcr:description}</p>
</sly>
```

O resultado será semelhante ao HTML abaixo, mas somente se houver uma propriedade `jcr:title` e uma propriedade `jcr:description` definidas, e se nenhuma delas estiver vazia:

```xml
<h1>MY TITLE</h1>
<p>MY DESCRIPTION</p>
```

Lembre-se de usar o elemento `sly` somente quando nenhum elemento existente puder ser anotado com a declaração de bloco. O motivo por trás dessa orientação é que os elementos `sly` bloqueiam o valor oferecido pela linguagem para não alterar o HTML estático ao torná-lo dinâmico.

Por exemplo, se o exemplo anterior já tivesse sido encapsulado dentro de um elemento `div`, o elemento `sly` adicionado seria abusivo:

```xml
<div>
    <sly data-sly-test="${properties.jcr:title && properties.jcr:description}">
        <h1>${properties.jcr:title}</h1>
        <p>${properties.jcr:description}</p>
    </sly>
</div>
```

E o elemento `div` poderia ter sido anotado com a condição:

```xml
<div data-sly-test="${properties.jcr:title && properties.jcr:description}">
    <h1>${properties.jcr:title}</h1>
    <p>${properties.jcr:description}</p>
</div>
```

### Comentários HTL {#htl-comments}

O exemplo a seguir mostra um comentário HTL na primeira linha e um comentário HTML na segunda linha.

```xml
<!--/* An HTL Comment */-->
<!-- An HTML Comment -->
```

Os comentários HTL são comentários HTML com uma sintaxe adicional semelhante a JavaScript. O processador ignora totalmente o comentário HTL e qualquer elemento contido nele, removendo-o do resultado.

No entanto, o conteúdo dos comentários HTML padrão é transmitido, e as expressões contidas no comentário são avaliadas.

Comentários HTML não podem conter comentários HTL e vice-versa.

### Contextos especiais {#special-contexts}

Para usar melhor o HTL, é importante entender bem as consequências de baseá-lo na sintaxe HTML.

Consulte a [seção Contexto de exibição](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#121-display-context) da especificação do HTL para obter mais detalhes.

### Nomes de elementos e atributos {#element-and-attribute-names}

As expressões só podem ser colocadas em um texto HTML ou em valores de atributo, mas não em nomes de elementos ou nomes de atributos, pois isso invalidaria o HTML. Para definir nomes de elementos dinamicamente, utilize a declaração `data-sly-element` nos elementos desejados; para definir nomes de atributos dinamicamente, e até mesmo configurar vários atributos de uma só vez, utilize a declaração `data-sly-attribute`.

```xml
<h1 data-sly-element="${myElementName}" data-sly-attribute="${myAttributeMap}">...</h1>
```

### Contextos sem declarações em bloco {#contexts-without-block-statements}

Como o HTL usa atributos de dados para definir declarações em bloco, não é possível definir essas declarações dentro dos seguintes contextos, onde somente expressões podem ser usadas:

* Comentários HTML
* Elementos de script
* Elementos de estilo

O motivo para isso é que o conteúdo desses contextos é texto e não HTML, e os elementos HTML contidos seriam considerados como dados de caracteres simples. Portanto, sem elementos HTML reais, também não é possível executar atributos `data-sly`.

Essa abordagem pode parecer uma restrição significativa. Porém, é a abordagem recomenda, visto que a linguagem de modelo HTML deve gerar apenas saídas em HTML válidas. Abaixo, a seção [Usar a API para acessar a lógica](#use-api-for-accessing-logic) mostra como a lógica adicional pode ser chamada a partir do modelo, o que pode ser útil para preparar saídas complexas para esses contextos. Para enviar dados do back-end a um script de front-end, gere uma string JSON com a lógica do componente e coloque-a em um atributo de dados usando uma expressão HTL simples.

O exemplo a seguir ilustra o comportamento de comentários HTML, mas o mesmo comportamento pode ser observado em scripts ou elementos de estilo:

```xml
<!--
    The title is: ${properties.jcr:title}
    <h1 data-sly-test="${properties.jcr:title}">${properties.jcr:title}</h1>
-->
```

O resultado é semelhante ao seguinte HTML:

```xml
<!--
    The title is: MY TITLE
    <h1 data-sly-test="MY TITLE">MY TITLE</h1>
-->
```

### Contextos explícitos necessários {#explicit-contexts-required}

Conforme explicado abaixo, na seção [Escape automático sensível ao contexto](#automatic-context-aware-escaping), um objetivo do HTL é reduzir os riscos de introdução de vulnerabilidades de script entre sites (XSS), aplicando automaticamente o escape com reconhecimento de contexto a todas as expressões. O HTL detecta o contexto de expressões na marcação HTML, mas não analisa CSS ou JavaScript em linha; portanto, desenvolvedores precisam especificar o contexto exato para essas expressões.

Não aplicar o escape correto resulta em vulnerabilidades XSS. Por isso, o HTL remove a saída de todas as expressões que estão em contextos de script e estilo, quando o contexto não foi declarado.

Este é um exemplo de como definir o contexto de expressões colocadas dentro de scripts e estilos:

```xml
<script> var trackingID = "${myTrackingID @ context='scriptString'}"; </script>
<style> a { font-family: "${myFont @ context='styleString'}"; } </style>
```

Para obter mais detalhes sobre como controlar o escape, consulte a seção [Contexto de exibição da linguagem de expressão](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#121-display-context) das especificações do HTL. 

## Recursos gerais do HTL {#general-capabilities-of-htl}

Esta seção aborda rapidamente os recursos gerais da Linguagem de modelo HTML.

### API de uso para acessar a lógica {#use-api-for-accessing-logic}

A API de uso do Java para a linguagem de modelo HTML (HTL) permite que um arquivo de HTL acesse métodos auxiliares em uma classe Java personalizada por meio do `data-sly-use`. Isso permite que toda lógica de negócios complexa seja encapsulada no código Java, enquanto o código HTL lida somente com a produção de marcação direta.

Consulte o documento [API de uso Java do HTL](java-use-api.md) para obter mais detalhes.

### Escape automático sensível ao contexto {#automatic-context-aware-escaping}

Considere o exemplo a seguir:

```xml
<p data-sly-use.logic="logic.js">
    <a href="${logic.link}" title="${logic.title}">
        ${logic.text}
    </a>
</p>
```

Na maioria das linguagens de modelo, esse exemplo poderia ocasionar uma vulnerabilidade de criação de script entre sites (XSS), pois mesmo quando todas as variáveis são automaticamente escapadas por HTML, o atributo `href` ainda deve ter um escape de URL específico. Essa omissão é um dos erros mais comuns, pois pode ser facilmente esquecida e é difícil de detectar de forma automatizada.

Para ajudar com isso, a linguagem de modelo HTML escapa automaticamente cada variável de acordo com o contexto no qual foi colocada. Essa abordagem é possibilitada pelo fato de que o HTL entende a sintaxe do HTML.

Considerando o seguinte arquivo `logic.js`:

```javascript
use(function () {
    return {
        link:  "#my link's safe",
        title: "my title's safe",
        text:  "my text's safe"
    };
});
```

O exemplo inicial resulta na seguinte saída:

```xml
<p>
    <a href="#my%20link%27s%20safe" title="my title&#39;s safe">
        my text&#39;s safe
    </a>
</p>
```

Observe como os dois atributos foram escapados de forma diferente, porque o HTL sabe que os atributos `href` e `src` devem ser escapados para o contexto do URI. Além disso, se o URI começasse com `javascript:`, o atributo teria sido totalmente removido, a menos que o contexto fosse explicitamente alterado para algo diferente.

Para obter mais detalhes sobre como controlar o escape, consulte a seção [Contexto de exibição da linguagem de expressão](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#121-display-context) das especificações do HTL.

### Remoção automática de atributos vazios {#automatic-removal-of-empty-attributes}

Considere o exemplo a seguir:

```xml
<p class="${properties.class}">some text</p>
```

Se o valor da propriedade `class` estiver vazio, a linguagem de modelo HTML removerá automaticamente todo o atributo `class` da saída.

Novamente, esse processo é possível porque o HTL entende a sintaxe do HTML e, portanto, mostra atributos com valores dinâmicos de maneira condicional, desde que o valor não esteja vazio. Isso é muito conveniente porque evita a adição de um bloco de condição ao redor dos atributos, o que tornaria a marcação inválida e ilegível.

Além disso, o tipo da variável colocada na expressão é importante:

* **Sequência de caracteres:**
   * **não vazio:** define a string como um valor de atributo.
   * **vazio:** remove o atributo completamente.

* **Número:** define o valor como um valor de atributo.

* **Booleano:**
   * **true:** exibe o atributo sem valor (como um atributo HTML booleano)
   * **false:** remove o atributo completamente.

Este é um exemplo de como uma expressão booleana permitiria controlar um atributo HTML booleano:

```xml
<input type="checkbox" checked="${properties.isChecked}"/>
```

Para definir atributos, a instrução `data-sly-attribute` também pode ser útil.

## Padrões comuns com HTL {#common-patterns-with-htl}

Esta seção apresenta alguns casos comuns. Ela explica a melhor forma de resolver esses casos com a linguagem de modelo HTML.

### Carregamento de bibliotecas de clientes {#loading-client-libraries}

No HTL, as bibliotecas do cliente são carregadas por meio de um modelo auxiliar fornecido pelo AEM, que pode ser acessado por meio de `data-sly-use`. Três modelos estão disponíveis nesse arquivo, que pode ser chamado por meio de `data-sly-call`:

* **`css`** - carrega somente os arquivos CSS das bibliotecas de clientes referenciadas.
* **`js`** - carrega somente os arquivos JavaScript das bibliotecas de clientes referenciadas.
* **`all`** - carrega todos os arquivos das bibliotecas de clientes referenciadas (CSS e JavaScript).

Cada modelo auxiliar espera uma opção `categories` para fazer referência às bibliotecas de clientes desejadas. Essa opção pode ser uma matriz de valores de string ou uma string contendo uma lista de valores separados por vírgula.

Veja a seguir dois pequenos exemplos.

#### Carregamento de várias bibliotecas de clientes de uma só vez {#loading-multiple-client-libraries-fully-at-once}

```xml
<sly data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html"
     data-sly-call="${clientlib.all @ categories=['myCategory1', 'myCategory2']}"/>
```

#### Referência a uma biblioteca do cliente em diferentes seções de uma página {#referencing-a-client-library-in-different-sections-of-a-page}

```xml
<!doctype html>
<html data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html">
    <head>
        <!-- HTML meta-data -->
        <sly data-sly-call="${clientlib.css @ categories='myCategory'}"/>
    </head>
    <body>
        <!-- page content -->
        <sly data-sly-call="${clientlib.js @ categories='myCategory'}"/>
    </body>
</html>
```

Neste exemplo, caso os elementos HTML `head` e `body` sejam colocados em arquivos diferentes, o modelo `clientlib.html` deverá ser carregado em cada arquivo que precisar dele.

A seção sobre as declarações de modelo e chamada na [especificação do HTL](specification.md) fornece mais detalhes sobre como a declaração e a chamada desses modelos funcionam.

### Enviar dados para o cliente {#passing-data-to-the-client}

A melhor e mais eficiente maneira de enviar dados para o cliente em geral, principalmente no caso de HTL, é usar atributos `data`.

O exemplo a seguir demonstra como serializar um objeto para JSON (também possível em Java) para transmitir ao cliente. Dessa forma, é possível inseri-lo facilmente em um atributo `data`:

```xml
<!--/* template.html file: */-->
<div data-sly-use.logic="logic.js" data-json="${logic.json}">...</div>
```

```javascript
/* logic.js file: */
use(function () {
    var myData = {
        str: "foo",
        arr: [1, 2, 3]
    };

    return {
        json: JSON.stringify(myData)
    };
});
```

A partir daí, é fácil imaginar como um JavaScript do lado do cliente pode acessar esse atributo e analisar novamente o JSON. Esta seria a abordagem em JavaScript correspondente à inserção em uma biblioteca de cliente, por exemplo:

```javascript
var elements = document.querySelectorAll("[data-json]");
for (var i = 0; i < elements.length; i++) {
    var obj = JSON.parse(elements[i].dataset.json);
    //console.log(obj);
}
```

### Trabalhar com modelos do lado do cliente {#working-with-client-side-templates}

Um caso especial, em que a técnica explicada na seção [Supressão das limitações de contextos especiais](#lifting-limitations-of-special-contexts) pode ser usada legitimamente, é escrever modelos do lado do cliente (como Handlebars, por exemplo) que estão localizados em elementos de `scrip`. Essa técnica poder ser usada com segurança nesse caso, porque o elemento de `script` não contém JavaScript como pressuposto, mas outros elementos HTML. Veja um exemplo de como isso funcionaria:

```xml
<!--/* template.html file: */-->
<script id="entry-section" type="text/template"
    data-sly-include="entry-section.html"></script>

<!--/* entry-section.html file: */-->
<div class="entry-section">
    <h2 data-sly-test="${properties.entrySectionTitle}">
        ${properties.entrySectionTitle}
    </h2>
    {{#each entry}}<h3><a href="{{link}}">{{title}}</a></h3>{{/each}}
</div>
```

A marcação do elemento `script` pode incluir declarações HTL em bloco sem contextos explícitos, pois o conteúdo do modelo Handlebars fica isolado em seu próprio arquivo. Além disso, este exemplo mostra como o HTL executado no lado do servidor (como no elemento `h2`) pode se misturar a uma linguagem de modelo executada no lado do cliente, como o Handlebars (mostrado no elemento `h3`).

Uma técnica mais moderna seria, no entanto, usar o elemento HTML `template`, que não exige o isolamento do conteúdo dos modelos em arquivos separados.

### Supressão das limitações de contextos especiais {#lifting-limitations-of-special-contexts}

Em casos especiais nos quais é necessário ignorar as restrições dos contextos de script, estilo e comentário, é possível isolar o conteúdo em um arquivo HTL separado. O HTL interpreta tudo em seu próprio arquivo como um fragmento de HTML padrão, ignorando qualquer contexto limitante de onde foi incluído.

Mais abaixo, consulte a seção [Trabalhar com modelos do lado do cliente](#working-with-client-side-templates) para obter um exemplo.

>[!CAUTION]
>
>Essa técnica pode apresentar vulnerabilidades de criação de script entre sites (XSS). Os aspectos de segurança precisam ser cuidadosamente estudados ao adotar essa abordagem. Geralmente, há melhores maneiras de implementar a mesma coisa do que confiar nessa prática.
