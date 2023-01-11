# Table of Contents
1. [Create Project](#1-create-project)
2. [Svelte Component/File Structure](#2-svelte-componentfile-structure)
3. [Reactivity](#3-reactivity)
4. [Html Text Content](#4-html-text-content)
5. [Assignments](#5-assignments)
6. [Array Mutation](#6-array-mutation)
7. [Object Mutation](#7-object-mutation)
8. [Child Component Prop Declaration](#8-child-component-prop-declaration)
9. [Spread Props](#9-spread-props)
10. [Logic Operators](#10-logic-operators)
11. [Ways of Dynamic Component Creation](#11-ways-of-dynamic-component-manupulation)
12. [$$restProps](#12-restprops)  
13. [Component Render Order](#13-component-render-order)  

#### Resources
- https://svelte.dev/docs
- https://sveltesociety.dev/cheatsheet
- https://sveltesociety.dev/components
- https://svelte.dev/examples/hello-world
- https://thevalleyofcode.com/svelte/



### 1. Create Project
```
yarn create vite my-app --template svelte-ts
```



### 2. Svelte Component/File Structure

```svelte 
<script>
</script>

<div/>

<style>
</style
```

### 3. Reactivity
```svelte
<script>
	let count = 0;
  
	$: doubled = count * 2;
  	$: if (count >= 10) {
		alert('count is dangerously high!');
		count = 9;
	}

	function handleClick() {
		count += 1;
	}
</script>

<h1>Hello {name.toUpperCase()}!</h1>

<button>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

### 4. Html Text Content
```svelte
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

### 5. Assignments
A simple rule of thumb: the updated variable must directly appear on the left hand side of the assignment.


### 6. Array Mutation
Array.Push does not mutate the array, it has to be reassigned to trigger the change
```svelte
<script>
	let numbers = [1, 2, 3, 4];

	function addNumber() {
		numbers = [...numbers, numbers.length + 1];
	}
    
    	//or
    
    	numbers[numbers.length] = numbers.length + 1;
    
    	//or
    
    	//Assign to itself after push
    	numbers.push(numbers.length + 1);
	numbers = numbers;	

	$: sum = numbers.reduce((t, n) => t + n, 0);
</script>
```



### 7. Object Mutation
```svelte
<script>
	let obj = { foo: { bar : { bip : 0} } };
	
	//Triggers Mutation
	obj.foo.bar += 1;

	//Doesn't Work - Original object reference is not used directly on the left side - Indirect
	const foo = obj.foo;
	foo.bar = 'baz';

	//Doesn't Work - Indirect
	function quox(thing) {
		thing.foo.bar = 'baz';
	}
	quox(obj);
	//But assigning obj back will trigger it
	obj = obj;
	
</script>
```

### 8. Child Component Prop Declaration
```svelte

//Nested.svelte
<script>
	export let answer;
	
	//or
	
	export let answer = 42; //Default Value
</script>


//App.svelte
<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42}/>
```

### 9. Spread Props
```svelte
<script>
	const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};
	
	<Info name={pkg.name} version={pkg.version} speed={pkg.speed} website={pkg.website}/>
	
	or 
	
	<Info {...pkg}/>
</script>
```

### 10. Logic Operators
```svelte
<script>
</script>

{#if user.loggedIn}
...
{:else}
...
{/if}


{#if x > 10}
...
{:else if 5 > x}
...
{:else}
...
{/if}


let cats = [
	{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
	{ id: 'z_AbfPXTKms', name: 'Maru' },
	{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
];
{#each cats as { id, name }, i}
	<li>
		<a target="_blank" href="https://www.youtube.com/watch?v={id}" rel="noreferrer">
			{i + 1}: {name}
		</a>
	</li>
{/each}


```

### 11. Ways of Dynamic Component Manupulation


A basic component example

```svelte
<script lang="ts">
  let count: number = 0
  export const increment = () => {
    count += 1
  }
</script>

<button on:click={increment}>
  count is {count}
</button>
```

Target must be provided at all times.

#### 1 - Create

```svelte
<script lang="ts">
  let counter:Counter;
  
  onMount(async() => {
    counter.increment();
  }
</script>

<div>
  <Counter bind:this={counter}/> //Prints: 1
</div>

```


#### 2 - Create

```svelte
<script lang="ts">
  let counter:typeof Counter;
  
  onMount(async() => {
    counter.increment();
  }
</script>

<div>
  <Counter bind:this={counter}/> //Prints: 1
</div>

```


#### 3 - Create

```svelte
<script lang="ts">
  let counter:typeof Counter; //or let counter: Counter
  
  onMount(async() => {
    var targetCounterDiv = document.querySelector('#counterDiv');

    counter = new Counter({
      target: targetCounterDiv,
      props: {}
    });
    counter.increment();
  }
</script>

<div id='counterDiv'>
  //Counter will be appended as last child of this div element
  //Also it will print 1 because of increment
</div>

```


#### 4 - Create

```svelte
<script lang="ts">
  let counter:typeof Counter; //or let counter: Counter
  
  onMount(async() => {
    var targetCounterDiv = document.querySelector('#counterDiv');

    counter = new Counter({
      target: targetCounterDiv,
      props: {}
    });
    counter.increment();
  }
</script>

<div id='counterDiv'>
  //Counter will be appended as last child of this div element
  //Also it will print 1 because of increment
</div>

```


#### 5 - Destroy

If you have reference to object

```svelte

let counter = new Counter({
  target: targetCounterDiv,
  props: {}
});

counter.$destroy();

```

#### Summary

- If you use new() 
  - Target is a must to provide
  - Instance is not equal to html component
  - No access to html element
  - Class methods are accessible and executable
  - If you bind it to svelte:component then check if the variable exist
   	 ```svelte
   	 {#if counter}
		<svelte:component this={counter} props={json formatted object}/>
	 {/if}
	```
  - You may have to use `tick()` before running any methods on the component
- If you don't create instance with new(), 
  - You can only use `<Counter bind:this={counter}/>`

- Suggested way is second option as much as possible or targeting into an html element such as div
- Example page: https://stackblitz.com/edit/vitejs-vite-x3sdxs?file=index.html,src%2FApp.svelte,src%2Flib%2FCounterWithId.svelte


### 12. $$restProps
Test


### 13. Component Render Order
Most inner component will throw onMount event first, then parent.


#### Example

A contains B - B contains C
Event order C-B-A

https://stackblitz.com/edit/vitejs-vite-wnpf7x?file=src%2FApp.svelte,src%2Flib%2FComponentC.svelte,src%2Flib%2FComponentB.svelte,src%2Flib%2FComponentA.svelte&terminal=dev


