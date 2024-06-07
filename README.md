# Svelte Quick Reference

#### Version 4.2.8

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

### Reactive Properties, Computed Properties and Side Effects ([more]()).
```html
<script>
  let count = 0; // Reactive properties
  $: doubled = count * 2; // Computed Properties
  $: console.log(`count ${count}`) // side effect runs when count changes
  $: {              // group statements
      ...
      ...
    }
  $: if(...) {} // this also possible
</script>
```

### If Else  ([more]()).
```html
{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}
```

### Loop  ([more]()).
```html
<!--                       key     index-->
{#each things as thing (thing.id), i} // i = (optional) current index
	<Thing name={thing.name}/>
{/each}
```

### Await  ([more]()).
```html
// brief
{#await promise then result}
	<p>The result is {result}</p>
{/await}

// Elaborated
{#await promise}
	<p>...waiting</p>
{:then result}
	<p>The result is {result}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

### Component Properties ([more]()).
```html
<script>
	export let answer = 'a mystery'; // default values
</script>

// Uses in parent component
<Nested answer={'a secret'}/>
```

### Component Event ([more]()).
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

### Event forwarding ([more]()).
```html
// Component.svelte
<script>
  ...
</script>
<button on:click>Click<button>

// Parent.svelte can now bind with click
<Component on:click={handleClick}/>
```