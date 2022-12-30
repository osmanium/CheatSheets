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


### Array Mutation
Array.Push does not mutate the array, it has to be reassigned to trigger the change
```svelte
<script>
	let numbers = [1, 2, 3, 4];

	function addNumber() {
		numbers = [...numbers, numbers.length + 1];
    
    or
    
    numbers[numbers.length] = numbers.length + 1;
    
    or
    
    //Assign to itself after push
    numbers.push(numbers.length + 1);
	  numbers = numbers;
	}

	$: sum = numbers.reduce((t, n) => t + n, 0);
</script>
```



### Object Mutation
```svelte

//Triggers Mutation
let obj = { foo: { bar : { bip : 0} } };
obj.foo.bar += 1;


//Doesn't Work - Original object reference is not used
const foo = obj.foo;
foo.bar = 'baz';

//Doesn't Work
thing.foo.bar = 'baz';


```

