<script>

	import { component_subscribe } from "svelte/internal";

</script>
<style>
    h1, h2, p {
        font-family: Arial, Helvetica, sans-serif;
    }
    pre {
        background-color: lightblue;
        padding: 8px;
    }

    blockquote {
        background-color: blanchedalmond;
        padding: 24px;
    }
</style>

<h1>Svelte School</h1>

<h2>Using Variables</h2>
<p>The first thing to know about Svelte is that the .svelete files are split into script, style, and html tags. Any variable defined in the script tag can be accessed via curly braces in the HTML tags:</p>

<pre><code>
    {`
    <script>
        const toy = 'skipping rope'
    </script>)
    <p>You found a {toy}</p> 
    `}
</code></pre>

<p>Will Return:</p>

<blockquote>You played with a skipping rope!</blockquote>

<h2>Style</h2>
<p>Style tags can be provided in each component to allow the component-level scoping of CSS:</p>
<pre><code>
    {`
    <style>
        h1 {
            font-size: 3em;
        }
    </style>
    <h1>This will be 3em tall, you don't need to see that</h1>
    `}
</code></pre>


<h2>Importing Components</h2>
<p>We can stick components together in much the same way we do it in other frameworks via use of Import.</p>
<pre><code>
    {`
    <script>
        import Counter from './Counter.svelte'
    </script>)
    `}
</code></pre>

This can be used in our HTML like so:

<blockquote>{`You have clicked me <Counter /> Times`}</blockquote>

<h2>HTML Tags</h2>

<p>So you might have HTML in a string that you want to render as HTML. This can be done via the use of the {`{@html ..} tag`} in curly braces</p>

<pre><code>
    {`
    <style>
     let example = '<p>I <em>love</em> eating fish!</p>'
    </style>

    {@html example}
    `}
</code></pre>



<h2>Making things Reactive</h2>
<p>A big part of web frameworks is making updates when th user interacts with the page. Svelte does this via the use of reactive variables. These are variables that are updated when the user interacts with the page. This is done via the use of the $: tag. This tag is used to define a block of code that will be run whenever the variable is updated.</p>
<p>These are super simple to use:</p>

<pre><code>
    {`
    <script>
        let count = 0
        function incrementCount() {
		count += 1;
	}
    </script>

    <button on:click>You have clicked me {count} times</button>
    `}
</code></pre>

<p>This will pretty much work as you expect. Compared to React this is so much less boilerplate</p>

<h2>Computed Variables</h2>
<p>Computed variables are variables that are derived from other variables. They are updated whenever the variables they are derived from are updated. This is done via the use of the $: tag. This tag is used to define a block of code that will be run whenever the variable is updated:</p>
<pre><code>
    {`
    <script>
        let count = 0
        function incrementCount() {
        count += 1;
    }
        $: doubleCount = count * 2
    </script>

    <button on:click>You have clicked me {count} times but double that is {doubleCount}</button>
    `}
</code></pre>

<p>We are not limited to reactively running values. we can run pretty much anything including multiple statements: </p>

<pre><code>
    <script>
        $: {
            console.log('The count is now', count)
            if (count > 10) {
                console.log('That is a lot of clicks')
            }
        }
    </script>
</code></pre>

<p>The reactivity only occurs when <strong>we are assigning. Mutations of arrays or objects do not trigger updates.</strong>If we add to an array it will not update unless we assign a variable, a typical pattern is to assign to itself:</p>

<pre><code>
    <script>
        let items = []
        function addItem() {
            items = [...items, 'new item']
        }
    </script>
</code></pre>

<p>While you can update an object or array's proerties like this too, we have a small gotcha in that we require the thing to be updated to be on the left hand side of the assignment. In other words:</p> 

<pre><code>
    <script>
        const car = { make: 'Ford', model: 'Fiesta' }
        car.make = 'Vauxhall' // This will update the car
        const carModel = car.model 
        carrModel = 'Corsa' // This will not update the car reactively
    </script>
</code></pre>

<h2>Props</h2>
<p>Props are variables that are passed into a component. They are defined in the script tag of the component and are passed in via the use of the {`<ComponentName prop="value" />`} syntax. Props are reactive and will update when the value is changed. They are defined as follows:</p>
<code><pre>
    {`
    <script>
        import Profile from './Profile.svelte'
    </script>

    <Profile fullName={fullName}/> // This will pass the fullName prop to the Profile component


    // In the Profile component
    <script>
        export let fullName = 'John Doe'
    </script>

    <h1>{fullName}</h1>
    `}
</pre></code>

<p>You'd need to export the prop to use it in the component_subscribe, weird I know. Using equals allows us to set a default value. We can also use the spread operator to pass an object's properties into a child component:</p>

<code><pre>
    {`
    <script>
        import Profile from './Profile.svelte'
        const user = {
            fullName: 'John Doe',
            age: 32,
            location: 'London'
        }
    </script>

    <Profile {...user}/>  // This will pass the fullName, age and location props to the Profile component in one go.`}
</pre></code>



<h2>Logic</h2>
We can conditionally render things in Svelte via the use of the {`{#if ..} tag`}:

<pre><code>
    {`
    <script>
       let favouriteSubject = 'french'
    </script>

    {#if favouriteSubject === 'french'}
        <p>Bon Jour!</p>
    {:else if favouriteSubject === 'spanish'}
        <p>Hola!</p>
    {:else}
        <p>Hello</p>
    {/if}
    `}
</code></pre>

<p>So the # is used to denote a block of code that will be run if the condition is true. The : is used to denote a block of code that is continuing. The / is used to denote the end of the block. We can also use the {`{#each ..} tag`} to loop over an array like so:</p>

<pre><code>
    {`
    <script>
        let subjects = ['french', 'spanish', 'english']
        let subjectDetails = [
		{subject: 'French', teacher: 'Alice'}, 
		{subject: 'English', teacher: 'Bob'}
	];
    </script>

    {#each subjects as subject}
        <p>{subject}</p>
    {/each}
    
    // We can access the index of the item in the array by using the {index} variable
    {#each subjects as subject, index}
        <p>{index] {subject}</p>
    {/each}

    // We can also use destructuring to access the properties of the object
    {#each subjectDetails as {subject, teacher}}
        <p>{subject} is taught by {teacher}</p>
    {/each}
    `}
</code></pre>

<p>Keyed each blocks are important as they allow Svelte to know what DOM Node to change. Without them, if oyu modify the value of an each block it will only occur at the end of the block. We can key an each block by simply adding a key attribute to the each block:</p>

<pre><code>
    {`
    {#each subjects as subject (subject.id)} // It is important that the key is unique and its generally better to use a string or a number rather than an object which can, in rare cases, be the same as another object.
        <p>{subject}</p>
    {/each}
    `}
</code></pre>

<p>Each can also be used for getting asynchronous data: </p>

<pre><code>
    {`
    <script>
        async function getSubjects() {
            const response = await fetch('https://example.com/subjects')
            const data =  response.json();
        if (response.ok) {
            return data;
        } else {
            throw new Error(data)
        }

        let promise = getSubjects()

        function handleClick() {
            promise = getSubjects()
        }
    </script>

    {#await promise}
      <p>Loading...</p>
    {:then subjects}
        {#each subjects as subject}
            <p>{subject}</p>
        {/each}
    {:catch error}
        <p>{error.message}</p>
   {/await}
    `}
</code></pre>


<h2>Events</h2>

<p>We have looked at on click before but of course there is a whole bunch of events we can deal with:</p>

<pre><code>
    {`
    <button on:click={handleClick}>Click me</button>
    <button on:mouseover={handleMouseOver}>Hover me</button>
    <button on:mouseout={e => console.log('I mouse outed!')}>Leave Me</button>
    <button on:keydown|once={handleKeyDown}>Press a key ...just only once</button>
    <form on:submit|preventDefault|once={handleFormSubmit}>...Form stuff here with chained events options</form>
    `}
</code></pre>

<p>What is cool is that components themselves can send their own events:</p>

<pre><code>
    {`
    // Child Component
    <script>
        import { createEventDispatcher } from 'svelte'
        const dispatch = createEventDispatcher() // Must be called as part of instantiation

        function handleClick() {
            dispatch('componentEvent', {message: 'Hello from the child component'})
        }
    </script>

    <button on:click={handleClick}>Click me</button>

    // Parent Component
    <script>
        import Child from './Child.svelte'
        <Child on:componentEvent={e => console.log(e.detail.message)}/>
    </script>
    `}
</code></pre>

<p>By default the component events only report up to the component above it. We can listen out for events in deeply nested events with a handy shortcut to bubble up the event accordingly:</p>

<pre><code>
    {`
    // MiddleComponent
    <script>
        import {BottomComponent} from 'svelte/internal' // Contains the event dispatcher
    </script>

    <BottomComponent on:componentEvent/>

    // TopComponent
    <script>
        import {MiddleComponent} from 'svelte/internal' // Inbetween Bottom and Top Components
    </script>

    <MiddleComponent on:componentEvent={e => console.log(e.detail.message)}/>
    `}
</code></pre>

<p>DOM events work a similar way if we have a button that has an <em>on:click</em> in a child component we can do something with the parent by simply forwarding that to the parent component:</p>

<pre><code>{`<CustomButton on:click={e => console.log('The button with an on:click event in the CustomButtom component was clicked')}`}</code></pre>

<h2>Bindings</h2>
<p>Typically w</p>