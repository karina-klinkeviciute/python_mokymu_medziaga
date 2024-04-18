# Exploring CSS Display, Text, and Fonts
Learning Objectives:

- Understand the display property and its values (block, inline, inline-block, none, etc.).
- Learn how to control text properties such as color, font-family, font-size, font-weight, text-align, and text-decoration.
- Explore different font options and how to integrate custom fonts into a webpage.

## CSS `display`

### 1. `block`

- **Description**: The `block` value for the CSS `display` property is used to render an element as a block-level element. Block-level elements typically start on a new line and stretch to fill the available width of their containing parent element.
  
- **Characteristics**:
  - Starts on a new line.
  - Occupies the full width available in its containing parent element, unless otherwise specified.
  - Can have dimensions (width, height) applied to it.
  - Allows other block-level and inline elements to be placed above or below it.

- **Examples of Block-Level Elements**:
  - `<div>`
  - `<p>`
  - `<h1>` to `<h6>`
  - `<ul>`, `<ol>`, `<li>`
  - `<header>`, `<footer>`, `<section>`, `<article>`

### 2. `inline`

- **Description**: The `inline` value for the CSS `display` property is used to render an element as an inline-level element. Inline-level elements do not start on a new line and only take up as much width as necessary.
  
- **Characteristics**:
  - Does not start on a new line; flows alongside adjacent elements.
  - Occupies only as much width as necessary to fit its content.
  - Does not accept width or height properties.
  - Allows other inline elements to be placed next to it on the same line.

- **Examples of Inline-Level Elements**:
  - `<span>`
  - `<a>`
  - `<strong>`, `<em>`
  - `<img>`
  - `<input>`, `<button>`

### 3. `inline-block`

- **Description**: The `inline-block` value for the CSS `display` property is a hybrid between `block` and `inline`. It allows an element to behave like an inline-level element while still retaining some properties of a block-level element.
  
- **Characteristics**:
  - Does not start on a new line; flows alongside adjacent elements.
  - Respects dimensions (width, height) specified by the CSS.
  - Allows other inline elements to be placed next to it on the same line.
  - Can have dimensions applied to it and respects margin and padding properties.

- **Use Cases**:
  - Often used for styling elements like buttons or navigation links that need to have block-level styling (such as dimensions and padding) but should still flow inline with text or other inline elements.


Hands-On Activity:

    Create a simple HTML file with multiple elements (e.g., <div>, <span>, <p>, <img>) and experiment with applying different display property values to each element using CSS.

Part 2: CSS Text Properties
Understanding Text Properties:

    Introduction to CSS text properties for styling text content on webpages.
    Explanation of common text properties:
        color
        font-family
        font-size
        font-weight
        text-align
        text-decoration
        line-height
        letter-spacing
        word-spacing, etc.
    Demonstration of how each property affects the appearance of text on a webpage.

Hands-On Activity:

    Modify the HTML file from Part 1 to include text content within different elements.
    Apply various text properties to the text content using CSS to achieve desired styling effects.
    Encourage students to experiment with different property values to see how they impact the appearance of text.

Part 3: CSS Fonts
Exploring Font Options:

    Introduction to CSS fonts and the importance of typography in web design.
    Overview of the default font families available in browsers.
    Explanation of how to specify custom font families using the font-family property.
    Introduction to web-safe fonts and Google Fonts as resources for accessing a wide range of fonts for web projects.

Hands-On Activity:

    Select a custom font from Google Fonts and integrate it into the HTML file from Part 2 using CSS.
    Experiment with applying the custom font to different elements and text within the webpage.
    Discuss the importance of font choice in enhancing the overall design and readability of a webpage.
