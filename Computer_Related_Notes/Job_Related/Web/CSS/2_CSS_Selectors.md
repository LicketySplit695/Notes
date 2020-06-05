# [Selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors)

#### **Definition**

It is a pattern of elements and other terms that tell the browser which HTML elements should be selected to have the CSS property values inside the rule applied to them. 

The element or elements which are selected by the selector are referred to as the *subject of the selector*. 



## [Selector Lists](https://developer.mozilla.org/en-US/docs/Web/CSS/Selector_list)

If we have same ruleset for more than one selectors, then they may be listed together by separating with a comma. For ex: 

Instead of 

```css
h1 { color: blue; } 
.special { color: blue; } 
```

We could write 

```css
h1, .special { color: blue; } 
```

However ==if any selector in the list is invalid then the whole group will be invalid.==



### Types of Selectors

There are different groups of selectors which are

#### [Universal Selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Universal_selectors)

The CSS universal selector `(*)` matches elements of any type. 

```css
* { style properties } 
```

`(*)` is optional for simple selectors, I.e. `*.warning` is same as `.warning` and `*#special` is same as `#special`. 

#### [Type](https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors) 

This group targets the HTML elements directly as an `<h1>`. 

```css
h1{ }
```

#### [Class](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors)

These are selectors that target a class 

```css
.box{ }
```

#### [ID](https://developer.mozilla.org/en-US/docs/Web/CSS/ID_selectors)

Selectors that target an ID 

```css
#unique{ }
```

#### [Attribute](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)

Gives ways to select an HTML element based on the presence of certain attribute in the element. For ex: 

```css
a[title] { } 
```

Or maybe if an attribute has a particular value 

```css
a[href="https://example.com"] { } 
```

---

##### Attribute Selector Syntax

`[attr]` 
Represents elements with an attribute name of attr. 

`[attr=value]` 
Represents elements with an attribute name of attr whose value is exactly value. 

`[attr~=value]` 
Represents elements with an attribute name of attr whose value is a whitespace-separated list of words, one of which is exactly value. 

`[attr|=value] `
Represents elements with an attribute name of attr whose value can be exactly value or can begin with value immediately followed by a hyphen, - (U+002D). It is often used for language subcode matches. 

`[attr^=value] for example a[href^="#"]` 
Represents elements with an attribute name of attr whose value is prefixed (preceded) by value. 

`[attr$=value]` 
Represents elements with an attribute name of attr whose value is suffixed (followed) by value. 

`[attr*=value] for example a[href*="example"]` 
Represents elements with an attribute name of attr whose value contains at least one occurrence of value within the string. 

`[attr operator value i] for example a[href*="insensitive" i]` 
Adding an i (or I) before the closing bracket causes the value to be compared case-insensitively (for characters within the ASCII range). 

`[attr operator value s] for example a[href*="cAsE" s] `
Adding an s (or S) before the closing bracket causes the value to be compared case-sensitively (for characters within the ASCII range). 

---



### [**Pseudo-classes**](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) **and** [**pseudo-elements**](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)

This group of selectors includes pseudo-classes, which style certain states of an element. For example `:hover` pseudo-class for example selects an element only when it is being hovered over by the mouse pointer: 

```css
a: hover { } 
```

Apart from above Pseudo-classes also let you apply a style to an element not only in relation to the content of the document tree, but also in relation to external factors like the history of the navigator ([`:visited`](https://developer.mozilla.org/en-US/docs/Web/CSS/:visited), for example) or the status of its content (like [`:checked`](https://developer.mozilla.org/en-US/docs/Web/CSS/:checked) on certain form elements). 

It also includes pseudo-elements, which select a certain part of an element rather than the element itself. For example 

```css
p::first-line { }
```

`::first-line` always selects the first line of text in the element acting like a `<span>` around the first line. 

A pseudo-element allows us to create items that do not normally exist in the document tree 



---

##### Note on Pseudo Elements

- In contrast to pseudo-elements, [pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/pseudo-classes) can be used to style an element based on its *state*. 

- You can use only one pseudo-element in a selector. It must appear after the simple selectors in the statement. 

---



## Combinators

These combine other selectors in order to target elements within our documents. 

### [Descendant Combinator ( )](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_combinator)

Typically represented by a single space ( ) character — combines two selectors such that element==s== matched by the second selector are selected if they have an ==**ancestor**== (parent, parent's parent, parent's parent's parent, etc) element matching the first selector. 

I.e. It selects nodes that are descendants of the first element. Ex. 

```css
/* List items that are descendants of the "my-things" list */ 
ul.my-things li { 
  margin: 2em; 
} 
```

### [Child Combinator (>)](https://developer.mozilla.org/en-US/docs/Web/CSS/Child_combinator)

The child combinator (>) is placed between two CSS selectors. It matches only those elements matched by the second selector that are the ==**direct children**== of elements matched by the first. 

I.e. It selects nodes that are direct children of the first element. Ex. 

```css
/* List items that are children of the "my-things" list */ 
ul.my-things > li { margin: 2em; }
```

---

#### Child Combinator is stricter than Descendant Combinator

Elements matched by the second selector must be the **immediate children** of the elements matched by the first selector. 

This is stricter than the descendant combinator, which matches all elements matched by the second selector for which there exists an ancestor element matched by the first selector, regardless of the number of "hops" up the DOM. 

---

### [General Sibling Combinator (~)](https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator)

The general sibling combinator (~) separates two selectors and matches the second element only if it ==**follows**== the first element (though not necessarily immediately), and both are children of the same parent element. Ex. 

```css
/* Paragraphs that are siblings of and 
   subsequent to any image */ 
img ~ p { 
  color: red; 
} 
```

### [Adjacent Sibling Combinator (+)](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator)

The adjacent sibling combinator (+) separates two selectors and matches the second element only if it ==**immediately**== **follows** the first element, and both are children of the same parent element. Ex. 

```css
/* Paragraphs that come immediately after any image */ 
img + p { 
  font-weight: bold; 
} 
```

### [Column Combinator (||)](https://developer.mozilla.org/en-US/docs/Web/CSS/Column_combinator) [^1]

The column combinator (||) is placed between two CSS selectors. It matches only those elements matched by the second selector that belong to the column elements matched by the first. Ex. 

```css
/* Table cells that belong to the "selected" column */ 
col.selected || td { 
  background: gray; 
}
```



---

[^1]: Browser Incompatibility should be checked