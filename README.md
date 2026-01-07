# No longer compatible

The get_current_component() internal API was removed in Svelte 5, and SvelteUI v0.14.0 still relies on it internally.

Budibase has since moved to svelte5. Most plugins can be upgraded using `budi plugins --migrate-svelte5`.

# Budibase Number Counter Input 

A number input with incremental steps, adapted from [svelteuidev](https://www.svelteui.org/core/number-input)

Set the default value, minimum, maximum, and the increment step. 

![step](https://user-images.githubusercontent.com/101575380/231453113-0d45b1ce-760a-4ab7-a61b-a01cd9699085.gif)

----

More on custom components: https://docs.budibase.com/docs/custom-plugin
