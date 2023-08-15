<script>
  import { getContext, onDestroy } from "svelte"
  import { NumberInput } from '@svelteuidev/core';
  import Label from "./Label.svelte";

  export let field
  export let label
  export let disabled = false
  export let defaultValue = 0
  export let onChange
  export let min = 0
  export let max
  export let step = 1

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");

  const formApi = formContext?.formApi;
  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: formField = formApi?.registerField(field, "number", defaultValue, disabled, null, formStep);
  
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
    if (parsed && !isNaN(parsed)) {
      return parsed
    }
    return 1
  }
</script>

<div class="spectrum-Form-item" use:styleable={$component.styles}>
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