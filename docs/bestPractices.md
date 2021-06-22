# Custom Element Best Practices

While writing custom elements, follow these best practices to make your element as robust and intuitive to use as possible.

## Reflect properties to attributes if that attribute is used as a styling hook

We shouldn't assume that consumers of the component will always pass data as attributes. Some frameworks (e.g. Svelte and Preact) prefer to pass data as properties. Because of that, if we are using the presence of an attribute as a styling hook (e.g. `:host[disabled]`), we should reflect the corresponding property to an attribute to ensure consistent behavior.

If there is no corresponding property, no additional configuration is required.

> You never know how a user will interact with your element. They might set a property in JavaScript, and then expect to read that value using an API like getAttribute(). If every attribute has a corresponding property, and both of them reflect, it will make it easier for users to work with your element. In other words, calling setAttribute('foo', value) should also set a corresponding foo property and vice versa. There are, of course, exceptions to this rule. You shouldn't reflect high frequency properties, e.g. currentTime in a video player. Use your best judgment. If it seems like a user will interact with a property or attribute, and it's not burdensome to reflect it, then do so. 

&mdash; [Google](https://developers.google.com/web/fundamentals/web-components/best-practices#aim-to-keep-primitive-data-attributes-and-properties-in-sync,-reflecting-from-property-to-attribute,-and-vice-versa.)


> We recommend reflecting from an attribute to a property, but to avoid reflecting from properties to attributes. This is because with custom elements properties can update often and triggering a DOM change for each update can impact performance.

&mdash; [Open-WC](https://open-wc.org/guides/knowledge/attributes-and-properties/#attribute-and-property-reflection)

### Additional reference
- Reference auro-badge [pull request](https://open-wc.org/guides/knowledge/attributes-and-properties/#attribute-and-property-reflection)
- [Impact on Svelte](https://css-tricks.com/using-custom-elements-in-svelte/#attributes-used-as-styling-hooks)

## If you query the children of a custom element, react to slot changes

In some custom elements (e.g. auro-accordion-group) we need to keep track of the children in that element's slot. When doing so, make sure you react to [slot changes](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement/slotchange_event) instead of only initializing once. This allows children to be dynamically added after the first time the element renders. Additionally, not doing this has [caused issues](https://github.com/AlaskaAirlines/auro-accordion/issues/18) in frameworks like Svelte that insert the parent into the DOM before the children.

### Additional reference
- Reference auro-accordion [pull request](https://github.com/AlaskaAirlines/auro-accordion/pull/21)

## Give boolean properties default values

When adding boolean properties to an element, give them a default value. Otherwise, the property will be initialized to `undefined`. This will usually work as expected, since `undefined` is falsy. However, it's more correct to explicitly set the property to `false`.

This has two main benefits:
1. If the property is retrieved from outside the element, it will return either `true` or `false` instead of `undefined`. Native DOM elements behave the same way, i.e. `document.querySelector('button').disabled` returns `false` instead of `undefined` if `disabled` is not set.
1. Svelte will understand that the property is a boolean and allow the consumer to write `<auro-button disabled>`. Otherwise, the consumer has to write `<auro-button disabled={true}>`.

### Additional reference
- [Svelte and boolean attributes](https://css-tricks.com/using-custom-elements-in-svelte/#boolean-attributes)
- Reference auro-badge [pull request](https://github.com/AlaskaAirlines/auro-badge/pull/11)


## Additional resources
- [Google's Custom Element Best Practices](https://developers.google.com/web/fundamentals/web-components/best-practices)
