# Markdown Showcase — All Element Types

This file demonstrates every common Markdown element so AEM's rendering pipeline can be validated end-to-end with site-specific styles applied.

---

## Table of Contents

- [Headings](#headings)
  - [Headings L2](#headingsl2)    
- [Paragraphs and Inline Text](#paragraphs-and-inline-text)
- [Blockquotes](#blockquotes)
- [Lists](#lists)
- [Task Lists](#task-lists)
- [Code](#code)
- [Tables](#tables)
- [Accordion — Details / Summary](#accordion--details--summary)
- [Links](#links)
- [Images](#images)
- [Horizontal Rules](#horizontal-rules)
- [Definition Lists](#definition-lists)
- [Footnotes](#footnotes)
- [Badges and Inline HTML](#badges-and-inline-html)
- [Math / Formulas](#math--formulas)
- [Emoji](#emoji)
- [Escape Characters](#escape-characters)

---

## Headings {#headings}

# Heading Level 1
## Heading Level 2
### Heading Level 3
#### Heading Level 4
##### Heading Level 5
###### Heading Level 6

Alternative H1 (Setext style)
==============================

Alternative H2 (Setext style) 
------------------------------

---

## Paragraphs and Inline Text 

This is a standard paragraph. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.

A second paragraph follows after a blank line.

**Bold text using double asterisks**

__Bold text using double underscores__

*Italic text using single asterisk*

_Italic text using single underscore_

***Bold and italic using triple asterisks***

___Bold and italic using triple underscores___

~~Strikethrough text~~

`Inline code` within a sentence.

Superscript: x^2^ (renderer-dependent)

Subscript: H~2~O (renderer-dependent)

This sentence ends with two trailing spaces for a hard line break.
This line follows immediately on a new line.

---

## Blockquotes 

> This is a single-level blockquote. It can span multiple lines and will be wrapped into a single block by the parser.

> **Blockquote with inline formatting**
> The quote continues here with *italic* and `code` embedded inline.

> Nested blockquotes:
> > Second level quote.
> > > Third level quote.

> ##### Blockquote with a heading inside
>
> - List item inside a blockquote
> - Another item
>
> Paragraph inside a blockquote.

---

## Lists

### Unordered Lists

- Item A
- Item B
  - Nested item B1
  - Nested item B2
    - Deeply nested B2a
    - Deeply nested B2b
- Item C

* Asterisk bullet item 1
* Asterisk bullet item 2

+ Plus bullet item 1
+ Plus bullet item 2

### Ordered Lists

1. First item
2. Second item
   1. Sub-item 2.1
   2. Sub-item 2.2
3. Third item
4. Fourth item — numbering in source doesn't need to be sequential; the parser orders it.

### Ordered List Starting at a Different Number

57. Starting at 57
58. Next item
59. Final item

### Mixed Nested Lists

1. Top-level ordered item one
   - Unordered sub-item
   - Another unordered sub-item
     1. Back to ordered at depth 3
     2. Second ordered at depth 3
2. Top-level ordered item two

---

## Task Lists

- [x] Design the markdown showcase file
- [x] Cover all heading levels
- [x] Include blockquotes, lists, and tables
- [ ] Validate rendering in AEM preview
- [ ] Apply site-specific CSS overrides
- [ ] Confirm accordion expand/collapse behaviour

---

## Code

### Inline Code

Use `npm install` to install dependencies. Refer to the `ApiSpecViewerModel.java` class for the Sling model implementation.

### Fenced Code Block — No Language

```
This is a plain fenced code block.
No syntax highlighting is applied.
Indentation and spacing are preserved exactly.
```

### JavaScript

```javascript
// apispecviewer.js — bootstrap logic
(function () {
  'use strict';

  document.addEventListener('DOMContentLoaded', function () {
    const containers = document.querySelectorAll('.devportal-apispecviewer');

    containers.forEach(function (container) {
      const specUrl   = container.dataset.specUrl;
      const renderer  = container.dataset.renderer || 'swagger';
      const hideTopBar = container.dataset.hideTopBar === 'true';
      const tryItOut  = container.dataset.tryItOut  === 'true';
      const deepLinking = container.dataset.deepLinking !== 'false';

      if (!specUrl) return;

      if (renderer === 'redoc') {
        Redoc.init(specUrl, {}, container);
      } else if (renderer === 'theneo') {
        const iframe = document.createElement('iframe');
        iframe.src = specUrl;
        iframe.style.width  = '100%';
        iframe.style.height = '100vh';
        iframe.style.border = 'none';
        container.appendChild(iframe);
      } else {
        // Default: Swagger UI
        SwaggerUIBundle({
          url:          specUrl,
          domNode:      container,
          presets:      [SwaggerUIBundle.presets.apis, SwaggerUIStandalonePreset],
          layout:       'StandaloneLayout',
          deepLinking:  deepLinking,
          tryItOutEnabled: tryItOut,
          plugins:      hideTopBar ? [] : [SwaggerUIBundle.plugins.DownloadUrl]
        });
      }
    });
  });
}());
```

### Java

```java
package com.allegion.core.sling.models;

import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

import javax.annotation.PostConstruct;

@Model(
    adaptables = Resource.class,
    defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ApiSpecViewerModel {

    @ValueMapValue
    private String specUrl;

    @ValueMapValue
    private String renderer;

    @ValueMapValue
    private String title;

    @ValueMapValue
    private boolean hideTopBar;

    @ValueMapValue
    private boolean tryItOut;

    @ValueMapValue
    private boolean deepLinking;

    @PostConstruct
    protected void init() {
        if (renderer == null || renderer.isEmpty()) {
            renderer = "swagger";
        }
        if (!deepLinking) {
            deepLinking = true; // default on
        }
    }

    public String getSpecUrl()    { return specUrl; }
    public String getRenderer()   { return renderer; }
    public String getTitle()      { return title; }
    public boolean isHideTopBar() { return hideTopBar; }
    public boolean isTryItOut()   { return tryItOut; }
    public boolean isDeepLinking(){ return deepLinking; }
}
```

### XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0"
          xmlns:cq="http://www.day.com/jcr/cq/1.0"
          xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:primaryType="cq:Component"
    jcr:title="API Spec Viewer"
    sling:resourceSuperType="core/wcm/components/commons/v1/template"
    componentGroup="DevPortal - Content"/>
```

### JSON

```json
{
  "apimBaseUrl": "https://my-apim.azure-api.net",
  "subscriptionKey": "$[secret:apim.subscription.key]",
  "exportFormat": "openapi+json",
  "allowedApiIds": [
    "petstore-v1",
    "payments-v2",
    "identity-v3"
  ]
}
```

### YAML

```yaml
openapi: "3.0.3"
info:
  title: Allegion DevPortal Sample API
  version: "1.0.0"
  description: |
    A sample OpenAPI spec used to validate the API Spec Viewer component.
servers:
  - url: https://api.allegion.com/v1
    description: Production
paths:
  /devices:
    get:
      summary: List all devices
      operationId: listDevices
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        "200":
          description: A list of devices
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/DeviceList"
components:
  schemas:
    DeviceList:
      type: array
      items:
        $ref: "#/components/schemas/Device"
    Device:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        status:
          type: string
          enum: [online, offline, unknown]
```

### Shell / Bash

```bash
# Build and deploy to a local AEM instance
mvn clean install -PautoInstallPackage \
  -Daem.host=localhost \
  -Daem.port=4502 \
  -Daem.user=admin \
  -Daem.password=admin

# Tail the AEM error log
tail -f crx-quickstart/logs/error.log | grep -i "ApiSpecViewer"
```

### HTML

```html
<div class="devportal-apispecviewer"
     data-spec-url="https://petstore3.swagger.io/api/v3/openapi.json"
     data-renderer="swagger"
     data-hide-top-bar="false"
     data-try-it-out="true"
     data-deep-linking="true">
  <div class="apispecviewer-container" id="apispecviewer-demo">
    <div class="apispecviewer-loading">
      <div class="apispecviewer-spinner"></div>
      <p>Loading API specification...</p>
    </div>
  </div>
</div>
```

### CSS

```css
/* API Spec Viewer — component styles */
.devportal-apispecviewer {
  position: relative;
  width: 100%;
  min-height: 400px;
}

.apispecviewer-loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4rem 0;
  color: var(--color-text-secondary, #666);
}

.apispecviewer-spinner {
  width: 48px;
  height: 48px;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-top-color: var(--color-brand-primary, #0057b8);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

### Diff

```diff
- import org.apache.http.impl.client.HttpClients;
+ import org.apache.hc.client5.http.impl.classic.HttpClients;
+ import org.apache.hc.client5.http.classic.methods.HttpGet;

  public class ApimSpecProxyServlet extends SlingSafeMethodsServlet {
-     try (CloseableHttpClient client = HttpClients.createDefault()) {
+     try (CloseableHttpClient client = HttpClients.createDefault()) {
          HttpGet get = new HttpGet(apimUrl);
```

---

## Tables

### Basic Table

| Column A | Column B | Column C |
|----------|----------|----------|
| Row 1, A | Row 1, B | Row 1, C |
| Row 2, A | Row 2, B | Row 2, C |
| Row 3, A | Row 3, B | Row 3, C |

### Alignment

| Left-aligned | Centre-aligned | Right-aligned |
|:-------------|:--------------:|--------------:|
| apple        |     banana     |        cherry |
| 1            |       2        |             3 |
| short        |    medium      |          long value here |

### Wider Reference Table

| Renderer | CDN Source                        | Try It Out | Deep Linking | Notes                                      |
|----------|-----------------------------------|:----------:|:------------:|--------------------------------------------|
| Swagger UI | `unpkg.com/swagger-ui-dist`     | Yes        | Yes          | Full interactive UI; largest bundle        |
| ReDoc    | `cdn.redoc.ly`                    | No         | Yes          | Documentation-first; three-panel layout    |
| Theneo   | Theneo hosted URL (iframe)        | —          | —            | Requires spec published on Theneo platform |

### Table with Inline Formatting

| Status | Badge | Description |
|--------|-------|-------------|
| `200 OK` | **Success** | Request completed successfully |
| `400 Bad Request` | *Client Error* | Missing or invalid parameter |
| `401 Unauthorized` | ~~Deprecated~~ **Auth Required** | Missing or invalid credentials |
| `403 Forbidden` | **Forbidden** | Authenticated but not authorised |
| `500 Internal Server Error` | `FATAL` | Unexpected server-side failure |

---

## Accordion — Details / Summary

Standard Markdown does not define an accordion element natively. Most renderers that support raw HTML will render `<details>` / `<summary>` as a collapsible section.

<details>
<summary><strong>What renderers does the API Spec Viewer support?</strong></summary>

The component supports three rendering engines:

1. **Swagger UI** — The industry-standard interactive documentation interface. Loaded from `unpkg.com`.
2. **ReDoc** — A cleaner, documentation-first layout with a three-panel design. Loaded from `cdn.redoc.ly`.
3. **Theneo** — Embeds a Theneo-hosted documentation URL inside a full-width `<iframe>`.

Select the renderer in the **Spec Configuration** tab of the authoring dialog.

</details>

<details>
<summary><strong>How is the subscription key kept secret?</strong></summary>

The subscription key is **never sent to the browser**. In Phase 2 of the component:

- The AEM Sling servlet (`ApimSpecProxyServlet`) reads the key from OSGi configuration.
- The key is stored encrypted using AEM Crypto Support or referenced via `$[secret:...]` syntax for AEM Cloud Service.
- The browser calls the AEM proxy path (`/bin/devportal/apim-spec?api=<id>`), not Azure APIM directly.
- The proxy attaches the `Ocp-Apim-Subscription-Key` header server-to-server.

```
Browser ──► AEM Proxy ──► Azure APIM
               (key added here, server-side)
```

</details>

<details>
<summary><strong>Why does "Try It Out" fail even though Swagger UI loads?</strong></summary>

Live API calls from the browser go directly to Azure APIM and are subject to **browser CORS restrictions**. The proxy only handles the spec fetch, not live API execution.

To fix:

- Add a CORS inbound policy to the APIM API or product.
- Allow the DevPortal domain as an `<origin>`.
- Ensure `OPTIONS` pre-flight requests are also allowed.

See the [CORS Configuration in Azure APIM](#cors-configuration-in-azure-apim) section of the API Spec Viewer documentation.

</details>

<details>
<summary>Nested accordion (renderer-dependent)</summary>

Outer accordion content.

<details>
<summary>Inner accordion</summary>

This is content inside a nested `<details>` block. Not all Markdown renderers support nesting.

</details>

</details>

---

## Links

### Inline Links

[Allegion DevPortal](https://www.allegion.com)

[Link with a title attribute](https://www.allegion.com "Allegion — Seamless Access")

### Reference-Style Links

This sentence uses a [reference-style link][ref-allegion] to keep the prose clean.

[ref-allegion]: https://www.allegion.com "Allegion"

### Automatic Links

<https://swagger.io>

<support@example.com>

### Relative Links

[API Spec Viewer documentation](./apispecviewer-component.md)

[Sling Filter authentication guide](./sling-filter-authentication.md)

[Authoring guide](./devportal-authoring-guide.md)

---

## Images

### Inline Image

![Swagger UI logo](https://validator.swagger.io/validator?url=https://petstore3.swagger.io/api/v3/openapi.json "Swagger Validator Badge")

### Reference-Style Image

![AEM logo][aem-logo]

[aem-logo]: https://www.adobe.com/favicon.ico "Adobe Experience Manager"

### Image with a Link

[![OpenAPI Initiative](https://www.openapis.org/wp-content/uploads/sites/3/2016/10/OpenAPI_Pantone.png)](https://www.openapis.org)

---

## Horizontal Rules

Three hyphens:

---

Three asterisks:

***

Three underscores:

___

---

## Definition Lists

Definition lists are supported by some extended Markdown processors (e.g., PHP Markdown Extra, Pandoc).

Swagger UI
:   An open-source JavaScript library that renders interactive OpenAPI documentation. Maintained by SmartBear.

ReDoc
:   A React-based OpenAPI documentation renderer with a clean three-column layout. Maintained by Redocly.

Theneo
:   A SaaS API documentation platform that generates interactive docs from an OpenAPI spec. Supports embedding via iframe.

AEM
:   Adobe Experience Manager. A Java-based CMS built on Apache Sling and OSGi, used to author and publish the DevPortal.

OSGi
:   Open Service Gateway Initiative. A modular Java framework used by AEM to manage components (bundles) at runtime.

---

## Footnotes

Footnotes are supported by extended processors such as GitHub Flavored Markdown and Pandoc.[^1]

The API Spec Viewer uses CDN-hosted libraries for Swagger UI and ReDoc.[^cdn]

Try It Out live calls bypass the AEM proxy and go directly to APIM.[^tryitout]

[^1]: Footnote rendering depends on the Markdown parser configured in your AEM or rendering pipeline.
[^cdn]: Ensure the AEM Content Security Policy (CSP) allows `unpkg.com` and `cdn.redoc.ly` as script and style sources.
[^tryitout]: This means CORS must be configured on Azure APIM for live call execution to succeed from the browser.

---

## Badges and Inline HTML

Raw HTML is supported in most Markdown renderers and allows richer elements not covered by standard Markdown syntax.

<p>
  <img alt="OpenAPI 3.0" src="https://img.shields.io/badge/OpenAPI-3.0-brightgreen?logo=openapi-initiative&logoColor=white">
  <img alt="AEM 6.5" src="https://img.shields.io/badge/AEM-6.5%2B-red?logo=adobe&logoColor=white">
  <img alt="License MIT" src="https://img.shields.io/badge/License-MIT-blue">
</p>

### Inline Coloured Spans (HTML)

Status legend: <span style="color:green">● Online</span> &nbsp; <span style="color:orange">● Degraded</span> &nbsp; <span style="color:red">● Offline</span>

### Keyboard Keys

Press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> to open the command palette in VS Code.

### Superscript and Subscript (HTML)

Chemical formula: H<sub>2</sub>O

Area formula: A = πr<sup>2</sup>

### Collapsible Table (HTML)

<details>
<summary>Expand to see the full OSGi property reference</summary>

<br>

| Property Key | Type | Default | Description |
|---|---|---|---|
| `apimBaseUrl` | String | — | Base URL of the Azure APIM instance |
| `subscriptionKey` | String (secret) | — | `Ocp-Apim-Subscription-Key` value |
| `exportFormat` | String | `openapi+json` | OpenAPI export format passed to APIM |
| `allowedApiIds` | String[] | `[]` (allow all) | Allowlist of API IDs the proxy will serve |

</details>

---

## Math / Formulas

Math rendering depends on the pipeline (e.g., KaTeX or MathJax integration). The delimiters below are the standard convention.

Inline math: $E = mc^2$

Block math:

$$
\int_{a}^{b} f(x)\,dx = F(b) - F(a)
$$

$$
\frac{\partial u}{\partial t} = h^2 \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} + \frac{\partial^2 u}{\partial z^2} \right)
$$

---

## Emoji

Emoji shortcode support is renderer-dependent (GitHub Flavored Markdown, Slack, etc.).

:white_check_mark: Task complete
:warning: Review required
:x: Blocked
:rocket: Deployment
:lock: Security
:gear: Configuration
:book: Documentation

---

## Escape Characters

The following characters have special meaning in Markdown and can be escaped with a backslash:

\\ — backslash
\` — backtick
\* — asterisk
\_ — underscore
\{ \} — curly braces
\[ \] — square brackets
\( \) — parentheses
\# — hash
\+ — plus
\- — minus / hyphen
\. — period
\! — exclamation mark
\| — pipe (in tables)

---

## Combined / Advanced Elements

### Blockquote Containing a Table

> **Supported renderers at a glance:**
>
> | Renderer | Bundle Size | Try It Out |
> |----------|-------------|:----------:|
> | Swagger UI | ~1.2 MB | Yes |
> | ReDoc | ~0.5 MB | No |
> | Theneo | 0 (iframe) | — |

### Code Block Inside a List

1. Install dependencies:

   ```bash
   npm install swagger-ui-dist redoc
   ```

2. Reference the dist files in your client library:

   ```xml
   <js>js/swagger-ui-bundle.js</js>
   <js>js/redoc.standalone.js</js>
   ```

3. Confirm the client library category matches the page policy:

   ```json
   { "categories": ["devportal.apispecviewer"] }
   ```

### Deeply Nested Blockquote with Code

> Phase 2 proxy flow:
>
> > Browser sends:
> > ```
> > GET /bin/devportal/apim-spec?api=petstore-v1
> > ```
> >
> > AEM proxy forwards:
> > ```
> > GET https://my-apim.azure-api.net/petstore-v1?export=true&format=openapi+json
> > Ocp-Apim-Subscription-Key: ****
> > ```

### Task List Inside a Blockquote

> Release checklist:
> - [x] OSGi config deployed to publish
> - [x] Allowlist updated with new API IDs
> - [ ] CSP headers verified in browser
> - [ ] Smoke test on production URL

---

*End of Markdown Showcase — all elements above are intended to exercise the full rendering pipeline.*
