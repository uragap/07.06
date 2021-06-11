---
"$title": Especificación de AMP para anuncios
order: '3'
formats:
  - anuncios
teaser:
  text: |2-

    _Si desea proponer cambios a la norma, comente el
    [Intención
toc: cierto
---

![1](https://prikolnye-kartinki.ru/img/picture/Jan/31/cd47b809864a288682ad43078a9642ab/1.jpg)

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/amp-a4a-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

*Si desea proponer cambios al estándar, comente sobre la [intención de implementar](https://github.com/ampproject/amphtml/issues/4264)* .

Los anuncios AMP HTML son un mecanismo para mostrar anuncios rápidos y de alto rendimiento en páginas AMP. Para garantizar que los documentos publicitarios AMP HTML ("creatividades AMP") se puedan procesar de forma rápida y fluida en el navegador y no degraden la experiencia del usuario, las creatividades AMP deben obedecer un conjunto de reglas de validación. De manera similar a las[reglas de formato de AMP](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml) , los anuncios AMP HTML tienen acceso a un conjunto limitado de etiquetas, capacidades y extensiones permitidas.

## Reglas de formato de anuncios AMP HTML<a name="amphtml-ad-format-rules"></a>

A menos que se especifique lo contrario a continuación, la creatividad debe obedecer todas las reglas dadas por las [reglas de formato de AMP](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml.html) , incluidas aquí como referencia. Por ejemplo, el anuncio HTML [de AMP Boilerplate se](#boilerplate) desvía del texto estándar de AMP.

Además, las creatividades deben cumplir las siguientes reglas:

<table>
<thead><tr>
  <th>Regla</th>
  <th>Razón fundamental</th>
</tr></thead>
<tbody>
<tr>
<td>Debe utilizar <code>&lt;html ⚡4ads&gt;</code> o <code>&lt;html amp4ads&gt;</code> como etiquetas adjuntas.</td>
<td>Permite a los validadores identificar un documento creativo como un documento AMP general o un documento publicitario HTML de AMP restringido y enviarlo de forma adecuada.</td>
</tr>
<tr>
<td>Debe incluir <code>&lt;script async src="https://cdn.ampproject.org/amp4ads-v0.js"&gt;&lt;/script&gt;</code> como secuencia de comandos en tiempo de ejecución en lugar de <code>https://cdn.ampproject.org/v0.js</code> .</td>
<td>Permite comportamientos de tiempo de ejecución personalizados para anuncios HTML de AMP que se muestran en iframes de origen cruzado.</td>
</tr>
<tr>
<td>No debe incluir una etiqueta <code>&lt;link rel="canonical"&gt;</code>
</td>
<td>Las creatividades de anuncios no tienen una "versión canónica que no sea de AMP" y no se indexarán en búsquedas de forma independiente, por lo que la autorreferencia sería inútil.</td>
</tr>
<tr>
<td>Puede incluir metaetiquetas opcionales en el encabezado HTML como identificadores, en el formato de <code>&lt;meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"&gt;</code> . Esas metaetiquetas deben colocarse antes del <code>amp4ads-v0.js</code> . El valor de <code>vendor</code> y <code>id</code> son cadenas que contienen solo [0-9a-zA-Z_-]. El valor de <code>type</code> es <code>creative-id</code> o <code>impression-id</code> .</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p>
<pre>
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474"&gt;
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"&gt;</pre>
</td>
</tr>
<tr>
<td>
<code>&lt;amp-analytics&gt;</code> solo puede orientar el selector de anuncios completo, a través de <code>"visibilitySpec": { "selector": "amp-ad" }</code> como se define en el <a href="https://github.com/ampproject/amphtml/issues/4018">número 4018</a> y el <a href="https://github.com/ampproject/amphtml/pull/4368">PR nº 4368</a> . En particular, no puede orientar a ningún selector de elementos dentro de la creatividad del anuncio.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br> <p>Example:</p> <pre>
&lt;amp-analytics id="nestedAnalytics"&gt;
  &lt;script type="application/json"&gt;
  {
    "requests": {
      "visibility": "https://example.com/nestedAmpAnalytics"
    },
    "triggers": {
      "visibilitySpec": {
      "selector": "amp-ad",
      "visiblePercentageMin": 50,
      "continuousTimeMin": 1000
      }
    }
  }
  &lt;/script&gt;
&lt;/amp-analytics&gt;
</pre> <p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p>
</td>
</tr>
</tbody>
</table>

### Calderería<a name="boilerplate"></a>

Las creatividades de anuncios HTML de AMP requieren una línea de estilo estándar diferente y considerablemente más simple que [los documentos generales de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) :

[sourcecode:html]
<style amp4ads-boilerplate>
  body {
    visibility: hidden;
  }
</style>
[/sourcecode]

*Justificación:* el `amp-boilerplate` oculta el contenido del cuerpo hasta que el tiempo de ejecución de AMP está listo y puede mostrarlo. Si Javascript está deshabilitado o el tiempo de ejecución de AMP no se carga, el texto estándar predeterminado garantiza que el contenido se muestre finalmente independientemente. Sin embargo, en los anuncios HTML de AMP, si Javascript está completamente deshabilitado, los anuncios HTML de AMP no se ejecutarán y nunca se mostrará ningún anuncio, por lo que no es necesaria la sección `<noscript>` En ausencia del tiempo de ejecución de AMP, la mayoría de la maquinaria en la que se basan los anuncios AMP HTML (por ejemplo, análisis para seguimiento de visibilidad o `amp-img` para visualización de contenido) no estará disponible, por lo que es mejor no mostrar ningún anuncio que uno que funcione mal.

Por último, el texto estándar del anuncio AMP HTML utiliza `amp-a4a-boilerplate` lugar de `amp-boilerplate` para que los validadores puedan identificarlo fácilmente y producir mensajes de error más precisos para ayudar a los desarrolladores.

Tenga en cuenta que se aplican las mismas reglas sobre mutaciones en el texto estándar que en el texto [estándar de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) .

### CSS<a name="css"></a>

<table>
<thead><tr>
  <th>Regla</th>
  <th>Razón fundamental</th>
</tr></thead>
<tbody>
  <tr>
    <td>
<code>position:fixed</code> y <code>position:sticky</code> están prohibidos en CSS creativo.</td>
    <td>
<code>position:fixed</code> rupturas de DOM en la sombra, de las que dependen los anuncios HTML de AMP. Además, los anuncios en AMP ya no pueden utilizar una posición fija.</td>
  </tr>
  <tr>
    <td>
<code>touch-action</code> está prohibida.</td>
    <td>Un anuncio que puede manipular <code>touch-action</code> puede interferir con la capacidad del usuario para desplazarse por el documento anfitrión.</td>
  </tr>
  <tr>
    <td>El CSS creativo está limitado a 20.000 bytes.</td>
    <td>Los grandes bloques de CSS inflan la creatividad, aumentan la latencia de la red y degradan el rendimiento de la página.</td>
  </tr>
  <tr>
    <td>La transición y la animación están sujetas a restricciones adicionales.</td>
    <td>AMP debe poder controlar todas las animaciones que pertenecen a un anuncio, de modo que pueda detenerlas cuando el anuncio no está en pantalla o los recursos del sistema son muy bajos.</td>
  </tr>
  <tr>
    <td>Los prefijos específicos del proveedor se consideran alias del mismo símbolo sin el prefijo a efectos de validación. Esto significa que si un símbolo <code>foo</code> está prohibido por las reglas de validación de CSS, entonces el símbolo <code>-vendor-foo</code> también estará prohibido.</td>
    <td>Algunas propiedades con prefijo de proveedor proporcionan una funcionalidad equivalente a las propiedades que de otro modo están prohibidas o restringidas por estas reglas.<br><br><p> Ejemplo: <code>-webkit-transition</code> y <code>-moz-transition</code> se consideran alias para la <code>transition</code> . Solo se permitirán en contextos donde <code>transition</code> simple (consulte la <a href="#selectors">sección Selectores a</a> continuación).</p>
</td>
  </tr>
</tbody>
</table>

#### Transiciones y animaciones CSS<a name="css-animations-and-transitions"></a>

##### Selectores<a name="selectors"></a>

Las `transition` y `animation` solo se permiten en selectores que:

- Contiene solo propiedades de `transition` , `animation` , `transform` , `visibility` u `opacity`

    *Justificación:* esto permite que el tiempo de ejecución de AMP elimine esta clase del contexto para desactivar animaciones, cuando sea necesario para el rendimiento de la página.

**Bien**

[sourcecode:css]
.box {
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

**Malo**

Propiedad no permitida en la clase CSS.

[sourcecode:css]
.box {
  color: red; // non-animation property not allowed in animation selector
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

##### Propiedades transicibles y animables<a name="transitionable-and-animatable-properties"></a>

Las únicas propiedades que se pueden cambiar son la opacidad y la transformación. ( [Justificación](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) )

**Bien**

[sourcecode:css]
transition: transform 2s;
[/sourcecode]

**Malo**

[sourcecode:css]
transition: background-color 2s;
[/sourcecode]

**Bien**

[sourcecode:css]
@keyframes turn {
  from {
    transform: rotate(180deg);
  }

  to {
    transform: rotate(90deg);
  }
}
[/sourcecode]

**Malo**

[sourcecode:css]
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
[/sourcecode]

### Extensiones y elementos integrados de AMP permitidos<a name="allowed-amp-extensions-and-builtins"></a>

Los siguientes son *módulos de extensión de AMP permitidos* y etiquetas integradas de AMP en una creatividad de anuncio HTML de AMP. Se prohíben las extensiones o etiquetas integradas que no se enumeren explícitamente.

- [amplificador de acordeón](https://amp.dev/documentation/components/amp-accordion)
- [amp-ad-exit](https://amp.dev/documentation/components/amp-ad-exit)
- [amp-analytics](https://amp.dev/documentation/components/amp-analytics)
- [amp-anim](https://amp.dev/documentation/components/amp-anim)
- [amp-animation](https://amp.dev/documentation/components/amp-animation)
- [amplificador de audio](https://amp.dev/documentation/components/amp-audio)
- [amp-bind](https://amp.dev/documentation/components/amp-bind)
- [carrusel de amplificador](https://amp.dev/documentation/components/amp-carousel)
- [amp-fit-text](https://amp.dev/documentation/components/amp-fit-text)
- [amp-font](https://amp.dev/documentation/components/amp-font)
- [forma de amplificador](https://amp.dev/documentation/components/amp-form)
- [amp-img](https://amp.dev/documentation/components/amp-img)
- [diseño de amplificador](https://amp.dev/documentation/components/amp-layout)
- [amp-lightbox](https://amp.dev/documentation/components/amp-lightbox)
- amp-mraid, de forma experimental. Si está considerando usar esto, abra un problema en [wg-ads](https://github.com/ampproject/wg-ads/issues/new) .
- [amp-bigote](https://amp.dev/documentation/components/amp-mustache)
- [amp-pixel](https://amp.dev/documentation/components/amp-pixel)
- [amplificador-observador-de-posición](https://amp.dev/documentation/components/amp-position-observer)
- [selector de amplificador](https://amp.dev/documentation/components/amp-selector)
- [amp-social-share](https://amp.dev/documentation/components/amp-social-share)
- [amp-video](https://amp.dev/documentation/components/amp-video)

La mayoría de las omisiones se deben al rendimiento o para facilitar el análisis de los anuncios HTML de AMP.

*Ejemplo:* `<amp-ad>` se omite de esta lista. Está explícitamente prohibido porque permitir un `<amp-ad>` dentro de un `<amp-ad>` podría generar cascadas ilimitadas de carga de anuncios, lo que no cumple los objetivos de rendimiento de los anuncios HTML de AMP.

*Ejemplo:* `<amp-iframe>` se omite de esta lista. No está permitido porque los anuncios podrían usarlo para ejecutar JavaScript arbitrario y cargar contenido arbitrario. Los anuncios que deseen utilizar estas capacidades deben devolver `false` en su [entrada a4aRegistry](https://github.com/ampproject/amphtml/blob/master/ads/_a4a-config.js#L40) y utilizar el mecanismo de representación de anuncios '3p iframe' existente.

*Ejemplo:* `<amp-facebook>` , `<amp-instagram>` , `<amp-twitter>` y `<amp-youtube>` se omiten por el mismo motivo que `<amp-iframe>` : todos crean iframes y pueden consumir recursos ilimitados. en ellos.

*Ejemplo:* `<amp-ad-network-*-impl>` se omiten de esta lista. La `<amp-ad>` maneja la delegación a estas etiquetas de implementación; las creatividades no deben intentar incluirlas directamente.

*Ejemplo:* `<amp-lightbox>` aún no está incluido porque incluso algunas creatividades de anuncios AMP HTML pueden procesarse en un iframe y actualmente no existe ningún mecanismo para que un anuncio se expanda más allá de un iframe. Se puede agregar apoyo para esto en el futuro, si se demuestra que lo desea.

### Etiquetas HTML<a name="html-tags"></a>

Las siguientes son *etiquetas permitidas* en una creatividad de anuncios AMP HTML. Las etiquetas no permitidas explícitamente están prohibidas. Esta lista es un subconjunto de la lista de [permitidos del apéndice de la etiqueta AMP](https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/../../spec/amp-tag-addendum.md) general. Al igual que esa lista, está ordenada de acuerdo con las especificaciones de HTML5 en la sección 4 [Los elementos de HTML](http://www.w3.org/TR/html5/single-page.html#html-elements) .

La mayoría de las omisiones se deben al rendimiento o a que las etiquetas no son estándar HTML5. Por ejemplo, `<noscript>` porque los anuncios HTML de AMP dependen de que JavaScript esté habilitado, por lo que un `<noscript>` nunca se ejecutará y, por lo tanto, solo aumentará la latencia y el ancho de banda de la creatividad y el costo. De manera similar, `<acronym>` , `<big>` , et al. están prohibidos porque no son compatibles con HTML5.

#### 4.1 El elemento raíz<a name="41-the-root-element"></a>

4.1.1 `<html>`

- Debe utilizar los tipos `<html ⚡4ads>` o `<html amp4ads>`

#### 4.2 Metadatos del documento<a name="42-document-metadata"></a>

4.2.1 `<head>`

4.2.2 `<title>`

4.2.4 `<link>`

- `<link rel=...>` no están permitidas, excepto para `<link rel=stylesheet>` .

- **Nota: a** diferencia de AMP general, las `<link rel="canonical">` están prohibidas.

    4.2.5 `<style>` 4.2.6 `<meta>`

#### 4.3 Secciones<a name="43-sections"></a>

4.3.1 `<body>` 4.3.2 `<article>` 4.3.3 `<section>` 4.3.4 `<nav>` 4.3.5 `<aside>` 4.3.6 `<h1>` , `<h2>` , `<h3>` , `<h4>` , `<h5>` y `<h6>` 4.3.7 `<header>` 4.3.8 `<footer>` 4.3.9 `<address>`

#### 4.4 Agrupar contenido<a name="44-grouping-content"></a>

4.4.1 `<p>` 4.4.2 `<hr>` 4.4.3 `<pre>` 4.4.4 `<blockquote>` 4.4.5 `<ol>` 4.4.6 `<ul>` 4.4.7 `<li>` 4.4.8 `<dl>` 4.4. 9 `<dt>` 4.4.10 `<dd>` 4.4.11 `<figure>` 4.4.12 `<figcaption>` 4.4.13 `<div>` 4.4.14 `<main>`

#### 4.5 Semántica a nivel de texto<a name="45-text-level-semantics"></a>

4.5.1 `<a>` 4.5.2 `<em>` 4.5.3 `<strong>` 4.5.4 `<small>` 4.5.5 `<s>` 4.5.6 `<cite>` 4.5.7 `<q>` 4.5.8 `<dfn>` 4.5. 9 `<abbr>` 4.5.10 `<data>` 4.5.11 `<time>` 4.5.12 `<code>` 4.5.13 `<var>` 4.5.14 `<samp>` 4.5.15 `<kbd >` 4.5.16 `<sub>` y `<sup>` 4.5.17 `<i>` 4.5.18 `<b>` 4.5.19 `<u>` 4.5.20 `<mark>` 4.5.21 `<ruby>` 4.5.22 `<rb>` 4.5.23 `<rt>` 4.5.24 `<rtc>` 4.5. 25 `<rp>` 4.5.26 `<bdi>` 4.5.27 `<bdo>` 4.5.28 `<span>` 4.5.29 `<br>` 4.5.30 `<wbr>`

#### 4.6 Ediciones<a name="46-edits"></a>

4.6.1 `<ins>` 4.6.2 `<del>`

#### 4.7 Contenido incrustado<a name="47-embedded-content"></a>

- El contenido incrustado solo se admite a través de etiquetas AMP, como `<amp-img>` o `<amp-video>` .

#### 4.7.4 `<source>`<a name="474-source"></a>

#### 4.7.18 SVG<a name="4718-svg"></a>

Las etiquetas SVG no están en el espacio de nombres HTML5. Se enumeran a continuación sin los identificadores de sección.

`&lt;svg&gt;``&lt;g&gt;``&lt;ruta&gt;``&lt;glifo&gt;``&lt;gliforef&gt;``&lt;marcador&gt;``&lt;vista&gt;``&lt;circulo&gt;``&lt;línea&gt;``&lt;polígono&gt;``&lt;polilínea&gt;``&lt;recto&gt;``&lt;texto&gt;``&lt;textpath&gt;``&lt;tref&gt;``&lt;tspan&gt;``&lt;clippath&gt;``&lt;filtro&gt;``&lt;gradiente lineal&gt;``&lt;radialgradient&gt;``&lt;mascara&gt;``&lt;patrón&gt;``&lt;vkern&gt;``&lt;hkern&gt;``&lt;defs&gt;``&lt;use&gt;``&lt;símbolo&gt;``&lt;desc&gt;``&lt;título&gt;`

#### 4.9 Datos tabulares<a name="49-tabular-data"></a>

4.9.1 `<table>` 4.9.2 `<caption>` 4.9.3 `<colgroup>` 4.9.4 `<col>` 4.9.5 `<tbody>` 4.9.6 `<thead>` 4.9.7 `<tfoot>` 4.9.8 `<tr>` 4.9. 9 `<td>` 4.9.10 `<th>`

#### 4.10 Formularios<a name="410-forms"></a>

4.10.8 `<button>`

#### 4.11 Secuencias de comandos<a name="411-scripting"></a>

- Al igual que un documento AMP general, la `<head>` la creatividad debe contener una etiqueta `<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>` .
- A diferencia de AMP general, `<noscript>` está prohibido.
    - *Justificación:* Dado que los anuncios HTML de AMP requieren que Javascript esté habilitado para funcionar, los `<noscript>` no sirven para nada en los anuncios HTML de AMP y solo cuestan el ancho de banda de la red.
- A diferencia de AMP general, `<script type="application/ld+json">` está prohibido.
    - *Justificación:* JSON LD se utiliza para el marcado de datos estructurados en las páginas de host, pero las creatividades de anuncios no son documentos independientes y no contienen datos estructurados. Los bloques JSON LD en ellos solo costarían ancho de banda de red.
- Todas las demás reglas y exclusiones de secuencias de comandos se transfieren de AMP general.
