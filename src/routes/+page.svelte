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


