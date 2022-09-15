---
weight: 6
title: "Code Block"
draft: false
---

## Code Blocks

### Single

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean sollicitudin lacinia varius. Maecenas quis velit id erat semper finibus non eget elit. Nam pharetra interdum velit, ut cursus ex pellentesque non.

```JS
const hello = function() {
    console.log("world");
}

hello();
```

### Tabbed

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean sollicitudin lacinia varius. Maecenas quis velit id erat semper finibus non eget elit. Nam pharetra interdum velit, ut cursus ex pellentesque non.

{{< tabs id="code-block" >}}

  {{< tab title="Javascript" >}}
  
   ```JS
   const hello = function() {
       console.log("world javascript");
   }

   hello();
   ```

  {{< /tab >}}

  {{< tab title="Typescript" >}}

   ```Typescript
   const hello = function() {
       console.log("world typescript");
   }

   hello();
   ```

  {{< /tab >}}

{{< /tabs >}}
