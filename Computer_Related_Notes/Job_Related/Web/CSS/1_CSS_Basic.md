# CSS Basics

#### Definition

**Cascading Style Sheets** (**CSS**) is a style sheet language used for describing the **presentation** of a document written in a **markup language** like HTML. 



## CSS Syntax

* The following defines a **CSS Ruleset**

  ![CSS Ruleset](CSS_Images/CSS_Basic_Ruleset.jpeg "Ruleset")

* CSS is developed by a group within *W3C* called the **CSS working group**. It is comprised of representatives from browser companies and other independent experts. 



### Default Browser Added Syntax

* Even if we don’t add our own CSS, the browser adds some default CSS to the document, as shown: 

  <img src="CSS_Images/CSS_Basic_DefaultCSS.jpeg" alt="Default Browser Added CSS" style="zoom: 85%;" />

* To reset the browser default CSS we may use [Eric Meyer's](https://meyerweb.com/eric/tools/css/reset/) CSS Reset.



---

#### Note on CSS Syntax

* If a property is unknown or if a value is not valid for a given property, the declaration is deemed *invalid* and is completely **ignored** by the browser's CSS engine. 

  * This is useful in a sense that if we want to use new CSS tricks that old browsers don’t understand, we can add the trick **after** the conventional approach, so that old browsers ignore the trick and new browsers overwrite the conventional code with the trick. 

* In CSS (and other web standards), US spelling has been agreed on as the standard to stick to where language uncertainty arises. 

---



## Adding CSS to HTML Document

1. **External CSS**

   Create a file *'style.css'* in styles folder and add the following line(s) within the `<head>` tag 

   ```html
   <link rel="stylesheet" type="text/css" href="styles/style.css"> 
   ```

2. **Internal CSS**

   Create CSS within the HTML Document in the `<head>` section within `<style>` tag. 

   ```html
   <head>  
       <style> 
           body { 
           	color: red; 
           } 
       </style> 
   </head> 
   ```

3.  **Inline CSS**

    Add the CSS Rules by separating with semi colons within corresponding HTML tags using the style attribute. 

   ```html
   <a style="color: blue;" href='http://www.example.com' title="Isn't this fun?">A link to my example.</a> 
   ```



### `@` rules

* They are language constructs beginning with an `@` symbol. For ex.: `@import` rules or `@media` queries.



---

#### Note

##### On priority

In the above 3 **Inline CSS** takes priority as the browser comes across it last. 

##### Use of Inline CSS

The following instances are examples where inline CSS may be used 

- In environment which is really restrictive (perhaps a CMS which allows to edit the HTML body). 
- It may be used a lot in HTML email in order to get compatibility with as many email clients as possible. 

##### Whitespaces in CSS

* The whitespace in CSS declarations separates values, but <u>property names never have whitespace</u>. 

---
