<script>
    import "@spectrum-css/fieldlabel/dist/index-vars.css"
  
    export let size = "M"
    export let muted
  </script>
  
    <label
      class:muted
      for=""
      class={`spectrum-FieldLabel spectrum-FieldLabel--size${size}`}
    >
      <slot />
    </label>
  
  <style>
    label {
      padding: 0;
      white-space: nowrap;
      color: var(--spectrum-global-color-gray-700);
    }
  
    .muted {
      opacity: 0.5;
    }
  </style>
  