# Table of Contents
1. [Create Project](#create-project)
2. [Svelte Component/File Structure](svelte-componentfile-structure)
3. [Reactivity](#reactivity)
4. [Html Text Content](#html-text-content)
5. [Assignments](#assignments)
6. [Array Mutation](#array-mutation)
7. [Object Mutation](#object-mutation)
8. [Child Component Prop Declaration](#child-component-prop-declaration)
9. [Spread Props](#spread-props)
10. [Logic Operators](#logic-operators)
11. 


### Create Project
```
yarn create vite my-app --template svelte-ts
```



### Svelte Component/File Structure

```svelte 
<script>
</script>

<div/>

<style>
</style
```

### Reactivity
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

### Html Text Content
```svelte
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

### Assignments
A simple rule of thumb: the updated variable must directly appear on the left hand side of the assignment.


### Array Mutation
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



### Object Mutation
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

### Child Component Prop Declaration
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

### Spread Props
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

### Logic Operators
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

