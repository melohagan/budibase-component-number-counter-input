<script>
  import { getContext, onDestroy } from "svelte"
  import { NumberInput, css } from '@svelteuidev/core';
  import Label from "./Label.svelte";

  export let field
  export let label
  export let disabled = false
  export let defaultValue = 0
  export let onChange
  export let min = 0
  export let max = 100
  export let step = 1

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");

  const formApi = formContext?.formApi;
  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: formField = formApi?.registerField(field, "number", parse(defaultValue), disabled, null, formStep);
  
  let fieldState
  let fieldApi

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
  });

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });

  const handleChange = e => {
    const changed = fieldApi.setValue(e.detail)
    if (onChange && changed) {
      onChange({ value: e.detail })
    }
  }
  const parse = (value) => {
    let parsed = parseFloat(value)
    if (parsed != null && !isNaN(parsed)) {
      return parsed
    }
    return 1
  }

  const styles = css({
      [`.svelteui-Input-input`]: {
        backgroundColor: "var(--spectrum-textfield-m-background-color, var(--spectrum-global-color-gray-50))",
        borderColor: "var(--spectrum-textfield-m-border-color, var(--spectrum-alias-border-color))",
        color: "var(--spectrum-textfield-m-text-color, var(--spectrum-alias-text-color))"
      },
      [`.svelteui-NumberInput-control`]: {
        borderColor: "var(--spectrum-textfield-m-border-color, var(--spectrum-alias-border-color))"
      },
      [`.svelteui-NumberInput-control:hover`]: {
        backgroundColor: "var(--spectrum-button-primary-m-background-color-hover, var(--spectrum-global-color-gray-800)) !important",
        borderColor: "var(--spectrum-button-primary-m-text-color-hover, var(--spectrum-global-color-gray-50)) !important"
      }
  })
</script>

<div class="spectrum-Form-item" use:styleable={$component.styles}>
    <div class={styles()}>
      <Label>{label}</Label>
      <NumberInput
          defaultValue={parse(defaultValue)}
          {disabled}
          min={parse(min)}
          max={parse(max)}
          precision={5}
          step={parse(step)}
          value={fieldState.value}
          stepHoldDelay={250}
          stepHoldInterval={50}
          on:change={handleChange}
        />
        </div>
</div>