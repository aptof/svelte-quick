# Svelte Quick Reference

#### Version 4.2.8 (JavaScript)

## Template Syntax

### Property Binding ([more](https://svelte.dev/docs/basic-markup)).
```html
<h1>Hello {name}!</h1>
<img src={src} alt="an image">
<img {src} alt="an image"> <!--Equivalent to above -->
```

### Two way binding ([more](https://svelte.dev/docs/element-directives#bind-property)).
```html
<input bind:value={name} />
<textarea bind:value={text} />
```

### `bind:group` ([more](https://svelte.dev/docs/element-directives#bind-group)).
```html
<script>
	let tortilla = 'Plain';

	/** @type {Array<string>} */
	let fillings = [];
</script>

<!-- grouped radio inputs are mutually exclusive -->
<input type="radio" bind:group={tortilla} value="Plain" />
<input type="radio" bind:group={tortilla} value="Whole wheat" />
<input type="radio" bind:group={tortilla} value="Spinach" />

<!-- grouped checkbox inputs populate an array -->
<input type="checkbox" bind:group={fillings} value="Rice" />
<input type="checkbox" bind:group={fillings} value="Beans" />
<input type="checkbox" bind:group={fillings} value="Cheese" />
<input type="checkbox" bind:group={fillings} value="Guac (extra)" />
```

### `bind:this` Get reference of an dom node ([more](https://svelte.dev/docs/element-directives#bind-this)).
```html
<script>
	/** @type {HTMLCanvasElement} */
	let canvasElement;
</script>

<canvas bind:this={canvasElement} />
```


### Class Binding ([more](https://svelte.dev/docs/element-directives#class-name)).
```html
<button class:active={isActive}>click</button>
<!-- Shorthand, for when name and value match -->
<div class:active>...</div>
<!-- Multiple class toggles can be included -->
<div class:active class:inactive={!active} class:isAdmin>...</div>
```

### Event binding ([more](https://svelte.dev/docs/element-directives#on-eventname)).
#### Dom Event
```html
<script>
  handleClick(event) {}
</script>
<button on:click={handleClick}>click</button>
```
#### Event modifier
```html
<form on:submit|preventDefault={handleSubmit}></form>
```
#### Event with arguments
```html
<button on:click={(e) => viewPost(id)}>View<button>
```

### Reactive Properties, Computed Properties and Side Effects ([more](https://svelte.dev/docs/svelte-components#script-3-$-marks-a-statement-as-reactive)).
```html
<script>
  let count = 0; <!-- Reactive properties -->
  $: doubled = count * 2; <!-- Computed Property -->
  $: console.log(`count ${count}`) <!-- side effect runs when count changes -->
  $: {              <!-- group statements -->
      ...
      ...
    }
  $: if(...) {}
</script>
```

### If Else  ([more](https://svelte.dev/docs/logic-blocks#if)).
```html
{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}
```

### Loop  ([more](https://svelte.dev/docs/logic-blocks#each)).
```html
<!--                       key     index -->
{#each things as thing (thing.id), i} // i = (optional) current index
	<Thing name={thing.name}/>
{/each}
```

### Await  ([more](https://svelte.dev/docs/logic-blocks#await)).
```html
{#await promise then result}
	<p>The result is {result}</p>
{/await}

<!-- Or -->
{#await promise}
	<p>...waiting</p>
{:then result}
	<p>The result is {result}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

### `#key` = destroys and recreates content when keyed value changes ([more](https://svelte.dev/docs/logic-blocks#key)).
```html
{#key value}
	<div transition:fade>{value}</div>
{/key}
```

### `@html` = put an html content ([more](https://svelte.dev/docs/special-tags#html)).
```html
<div class="blog-post">
	<h1>{post.title}</h1>
	{@html post.content}
</div>
```

### `@debug` = Debug with devtools ([more](https://svelte.dev/docs/special-tags#debug)).
```html
<script>
	let user = {
		firstname: 'Ada',
		lastname: 'Lovelace'
	};
</script>

{@debug user} <!-- Pause whenever user changes -->

<h1>Hello {user.firstname}!</h1>
```

### `@const` Declare a local const inside markup ([more](https://svelte.dev/docs/special-tags#const)).
```html
<script>
	export let boxes;
</script>

{#each boxes as box}
	{@const area = box.width * box.height}
	{box.width} * {box.height} = {area}
{/each}

{@const} is only allowed as direct child of {#if}, {:else if}, {:else}, {#each}, {:then}, {:catch}, <Component /> or <svelte:fragment />.
```

### Component Properties ([more]()).
```html
<script>
	export let answer = 'a mystery'; // default values
</script>

// Uses in parent component
<Nested answer={'a secret'}/>
```

### $$props = all the props that are passed to a component ([more](https://svelte.dev/docs/basic-markup#attributes-and-props)).
```html
<Widget {...$$props}/>
```

### $$restProps = all the props that are declared as export ([more](https://svelte.dev/docs/basic-markup#attributes-and-props)).
```html
// CustomInput.svelte
<script>
  ...
</script>
<input {...$$restProps} />
```

### Component Event ([more](https://svelte.dev/docs/component-directives#on-eventname)).
```html
// Component.svelte
<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  function sayHello() {
    dispatch('message', {
      text: 'Hello!'
    });
  }
</script>

<button on:click={sayHello}>
	Click to say hello
</button>

// Parent.svelte
<Component on:message={handleMessage}>
```

### Event forwarding ([more](https://svelte.dev/docs/component-directives#on-eventname)).
```html
// Component.svelte
<script>
  ...
</script>
<button on:click>Click<button>

// Parent.svelte can now bind with click
<Component on:click={handleClick}/>
```

### `slot` ([more](https://svelte.dev/docs/special-elements#slot)).
```html
<!-- Widget.svelte -->
<div>
  <slot name="header">No header was provided</slot>
  <slot>Default</slot>
  <slot name="footer" />
</div>

<!-- App.svelte -->
<Widget>
  <h1 slot="header">Hello</h1>
  <p>Content for default slot</p>
  <p slot="footer">Copyright (c) 2019 Svelte Industries</p>
</Widget>
```

### `$$slots` Used to check if a named slot is passed by parent ([more](https://svelte.dev/docs/special-elements#slot-$$slots)).
```html
<!-- Card.svelte -->
{#if $$slots.description}
  <!-- rendered if provided by parent -->
  <slot name="description" />
{/if}
```

### Slot Key ([more](https://svelte.dev/docs/special-elements#slot-slot-key-value)).