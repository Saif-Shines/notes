# Visual Studio Code

# Markdown

- The Cheatsheet of md
    
    ```markdown
    <img align='right' height=100 src='../../../public/vscode.png'>
    
    # Using Visual Studio Code
    
    * 📄 **Awesome Documents**
    * ⏩ [Emmet](./emmet.md)
    * 🎛 [Refactoring](./refactoring.md)
    * ✅ [Type-Checking](./type-checking.md)
    * 🐞 [Debugging](./debugging.md)
    
    ---
    
    ## Images
    
    Normal markdown images
    ```md
    ![VS Code](../../public/vscode.png)
    ```
    > ![VS Code](../../public/vscode.png)
    
    The HTML [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) tag can be used as well. Many attributes will be respected in most markdown renderers (including GitHub)
    
    ```html
    <img src="../../public/vscode.png" height=50 align=right vspace=20/>
    <img src="../../public/vscode.png" height=100/>
    ```
    > <img src="../../public/vscode.png" height=50 align=right vspace=20/>
    > <img src="../../public/vscode.png" height=100/>
    
    👉 Keep in mind that, in this case, GitHub wraps each image in
    ```html
    <p>
      <a href="./myimage.jpg">
        <!--image-->
      </a>
    </p>
    ```
    
    ## Alignment
    
    The `align` attribute can be used on a variety of HTML tags
    ```html
    <p align=right>right ➡</p>
    <p align=center>⬅ center ➡</p>
    <p align=left>⬅ left</p>
    ```
    > <p align=right>right ➡</p>
    > <p align=center>⬅ center ➡</p>
    > <p align=left>⬅ left</p>
    
    If you wrap multiple inline elements in a `<p>` tag, it's possible to do some interesting things with vertical `align` attribute values
    
    ```html
    <p>
      <img src="../../public/vscode.png" height=50 align=top />
      <img src="../../public/vscode.png" height=50 align=bottom />
      <img src="../../public/vscode.png" height=100/>
    </p>
    ```
    > <p>
    >   <img src="../../public/vscode.png" height=50 align=top />
    >   <img src="../../public/vscode.png" height=50 align=bottom />
    >   <img src="../../public/vscode.png" height=100/>
    > </p>
    
    ## Lists
    
    ### Unordered Lists
    
    These can be nested to create a multi-level outline
    
    ```md
    * one
    * two
      * three
        * four
    ```
    > * one
    > * two
    >   * three
    >     * four
    
    ### Ordered Lists
    
    These cannot be nested in most markdown rendering engines
    
    ```md
    1. first
    1. second
    1. third
    ```
    > 1. first
    > 1. second
    > 1. third
    
    but, if you use HTML, you can fix this, and customize the list "type"
    
    ```html
    <ol type='A' >
      <li> first </li>
      <li> second </li>
      <li> third</li>
      <ol type='a' >
        <li> fourth </li>
        <ol type='i' >
          <li> fifth </li>
        </ol>
      </ol>
    </ol>
    ```
    > <ol type='A' >
    >   <li> first </li>
    >   <li> second </li>
    >   <li> third</li>
    >   <ol type='a' >
    >     <li> fourth </li>
    >     <ol type='i' >
    >       <li> fifth </li>
    >     </ol>
    >   </ol>
    > </ol>
    
    ### [Description Lists](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl) (`<dl>`)
    
    ```html
    <dl>
      <dt>Images</dt>
      <dd>.jpg, .gif, .png</dd>
      <dt>Styles</dt>
      <dd>.css</dd>
      <dt>Scripts</dt>
      <dd>.js</dd>
      <dt>Documents</dt>
      <dd>.html</dd>
    </dl>
    ```
    
    > <dl>
    >   <dt>Images</dt>
    >   <dd>.jpg, .gif, .png</dd>
    >   <dt>Styles</dt>
    >   <dd>.css</dd>
    >   <dt>Scripts</dt>
    >   <dd>.js</dd>
    >   <dt>Documents</dt>
    >   <dd>.html</dd>
    > </dl>
    
    ## [Details](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)/Summary
    
    ````html
    <details>
      <summary>Click me for something absolutely amazing</summary>
    
    ```ts
      let Hello : string = 'World';
    ```
    </details>
    ````
    
    > <details>
    >  <summary>Click me for something absolutely amazing</summary>
    >
    >```ts
    >  let Hello : string = 'World';
    >```
    > </details>
    
    | Item      | Price | Quantity |
    |-----------|-------|----------|
    | 🍇 Grapes | $2.99 | 3        |
    | 🍐 Pears  | $4.15 | 1        |
    | 🍋 Lemons | $0.99 | 2        |
    
    > ```md
    > | Item      | Price | Qty |
    > |-----------|-------|-----|
    > | 🍇 Grapes | $2.99 | 3   |
    > | 🍐 Pears  | $4.15 | 1   |
    > | 🍋 Lemons | $0.99 | 2   |
    > ```
    
    ---
    #### Thanks to folks who posted tips I didn't know about!
    * [@mxstbr](https://github.com/mxstbr)  for [hanging indentation](https://github.com/mxstbr/github-markdown-tricks#hanging-indendation)
    
    ---
    NEXT: ⏩ [Emmet](./emmet.md)
    ```
    
- emmet
    
    ```markdown
    <img align='right' height=100 src='../../../public/vscode.png'>
    
    # Using Visual Studio Code
    
    * 📄 [Awesome Documents](./markdown.md)
    * ⏩ **Emmet**
    * 🎛 [Refactoring](./refactoring.md)
    * ✅ [Type-Checking](./type-checking.md)
    * 🐞 [Debugging](./debugging.md)
    
    ---
    
    ## Emmet Autocompletions
    
    * Speed up repetitive tasks with `TAB`
    * Plugins for most editors
    * Built into VS Code
    * Works in JSX
    
    <br><br><br><br>
    
    ### Emmet: HTML Elements
    * Think of as CSS selectors for DOM generation
    * Cursor usually ends up placed just where you want it to be next
    
    ##### `div`
    ```html
    <div></div>
    ```
    
    ##### `.foo`
    ```html
    <div class="foo"></div>
    ```
    
    <br><br><br><br>
    
    ##### `span.foo#bar`
    ```html
    <span class="foo" id="bar"></span>
    ```
    
    ##### `img`
    ```html
    <img src="" alt="">
    ```
    
    <br><br><br><br>
    
    ### Nesting & Sibling
    * Just like in CSS, we can use...
    
    | Selector | Description |
    |-----|------|
    | `>` | Direct decendant selector |
    | `+` | Sibling selector |
    
    ##### `ul>li.special-item`
    ```html
    <ul>
      <li class="special-item"></li>
    </ul>
    ```
    
    ##### `.navbar+.content+.footer`
    ```html
    <div class="navbar"></div>
    <div class="content"></div>
    <div class="footer"></div>
    ```
    
    We can also use the "climb up" operator: `^`
    
    ##### `div+div>p>span+em^bq`
    ```html
    <div></div>
    <div>
      <p><span></span><em></em></p>
      <blockquote></blockquote>
    </div>
    ```
    
    Personally, I prefer using parentheses (grouping) rather than climb-up
    
    ##### `div+div>(p>(span+em))+bq`
    ```html
    <div></div>
    <div>
      <p><span></span><em></em></p>
      <blockquote></blockquote>
    </div>
    ```
    
    There's a feature called "multiplication" that's useful for lists
    
    ##### `ul.col>li.col-item*5`
    ```html
    <ul class="col">
      <li class="col-item"></li>
      <li class="col-item"></li>
      <li class="col-item"></li>
      <li class="col-item"></li>
      <li class="col-item"></li>
    </ul>
    ```
    
    And you can even add text to the body of elements, with a placeholder for number in the list
    
    ##### `ul.col>li.col-item*5{Item $}`
    ```html
    <ul class="col">
      <li class="col-item">Item 1</li>
      <li class="col-item">Item 2</li>
      <li class="col-item">Item 3</li>
      <li class="col-item">Item 4</li>
      <li class="col-item">Item 5</li>
    </ul>
    ```
    
    <br><br><br><br>
    
    ### Emmet: Styles
    * Lots of different combinations (too many to remember)
    * Value aliases are pretty easy to get used to
    
    | Alias | Value |
    |-------|-------|
    | `p`   | Percent |
    | `e`   | em |
    | `r`   | rem |
    | `px`   | px |
    
    * Rule of thumb: first letter of each word for a property
    
    | Emmet | Evaluates to |
    |-------|-------|
    | `w100p!`   | `width: 100% !important` |
    | `lh1.5r`   | `line-height: 1.5rem` |
    | `o50p`   | `opacity: 50%` |
    
    <br><br><br><br>
    
    ### Tips for Productive Emmet Use
    * No newlines or spaces
    * You can create custom shortcuts (more on this later)
    * Aim for several small expansions, rather than one huge expansion
    * Start basic, and add new shortcuts over time
    
    <br><br><br><br>
    
    # Exercise 1: Rapid Expansion
    <img align=right width=150 src='../../public/emmet/click-for-more.png'>
    
    >  * We want to add a new tile to the right of each category row, as shown here 👉
    >  * You'll need to edit the `render` function in [client/routes/home/category-row/index.tsx](../../client/routes/home/category-row/index.tsx)
    >  * There's a per-category image you can use, which take the form `/images/fallback-<lowercase-category-name>.png`. (i.e., [http://localhost:3000/images/fallback-frozen.png](http://localhost:3000/images/fallback-frozen.png))
    > * Use Emmet autocompletions to build the appropriate HTML structure
    > ```html
    > <li class="GroceryItem mui-panel">
    >   <h4 class="item-name">
    >     Click Here for More
    >   </h4>
    >   <span class="click-for-more">              
    >     <img class='item-image' src=? />
    >   </span>
    > </li>
    > ```
    > * ⚠️ React likes `className=` not `class=`
    > * ⚠️ You interpolate dynamic and static values in JSX like this
    > ````js
    > <img alt={`my value is ${3 + 4}`} />
    > ````
    
    ---
    NEXT: 🎛 [Refactoring](./refactoring.md)
    ```
    
-