<script context="module">
  import _prism from "./import";
  export const prism = _prism;
  export const highlight = _prism.highlightElement;
  export const globalConfig = { transform: (x) => x };
</script>

<script>
  import "prism-svelte";
  import { tick } from "svelte";

  export let language = "javascript";
  export let source = "";
  export let transform = (x) => x;
  let element, formattedCode;

  $: $$props && (source || element) && highlightCode();

  function highlightCode() {
    const grammar = prism.languages[language];
    let body = source || element.textContent;
    body = globalConfig.transform(body);
    body = transform(body);
    formattedCode =
      language === "none" ? body : prism.highlight(body, grammar, language);
  }
</script>

<code bind:this={element} style="display:none">
  <slot />
</code>

<pre class="language-{language}" command-line data-output="2-17"><code class="language-{language}">{#if language === 'none'}{formattedCode}{:else}{@html formattedCode}{/if}</code></pre>
