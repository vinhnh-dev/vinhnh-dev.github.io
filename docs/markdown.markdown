---
layout: page
title: Markdown Cheat Sheet
tagline: Markdown
permalink: /markdown/
parent: Miscellaneous
nav_order: 2
---

Source: <https://www.markdownguide.org/basic-syntax/>

## Overview

Nearly all Markdown applications support the basic syntax outlined in the original Markdown design document. There are minor variations and discrepancies between Markdown processors.

## Headings

To create a heading, add number signs (`#`) in front of a word or phrase. The number of number signs you use should correspond to the heading level.

```markdown
# Heading level 1
## Heading level 2
### Heading level 3
```

### Heading best practices

For compatibility, always put a space between the number signs (`#`) and the heading name.

```markdown
# Here's a Heading
```

```markdown
#Here's a Heading
```

You should also put blank lines before and after a heading for compatibility.

```markdown
Try to put a blank line before...

# Heading

...and after a heading.
```

## Paragraphs

To create paragraphs, use a blank line to separate one or more lines of text.

```markdown
I really like using Markdown.

I think I'll use it to format all of my documents from now on.
```

### Paragraph best practices

Unless the paragraph is in a list, don’t indent paragraphs with spaces or tabs.

```markdown
Don't put tabs or spaces in front of your paragraphs. Keep lines left-aligned like this.
```

## Line Breaks

To create a line break or new line (`<br>`), end a line with two or more spaces, and then type return.

```markdown
This is the first line.  
And this is the second line.
```

### Line break best practices

Trailing whitespace can be hard to see in an editor. If your Markdown application supports HTML, you can use the `<br>` HTML tag.

```markdown
First line with the HTML tag after.<br>
And the next line.
```

## Emphasis

### Bold

```markdown
I just love **bold text**.
I just love __bold text__.
Love**is**bold
```

### Italic

```markdown
Italicized text is the *cat's meow*.
Italicized text is the _cat's meow_.
A*cat*meow
```

### Bold and Italic

```markdown
This text is ***really important***.
This text is ___really important___.
This text is __*really important*__.
This text is **_really important_**.
This is really***very***important text.
```

## Blockquotes

To create a blockquote, add a `>` in front of a paragraph.

```markdown
> Dorothy followed her through many of the beautiful rooms in her castle.
```

### Blockquotes with multiple paragraphs

```markdown
> Dorothy followed her through many of the beautiful rooms in her castle.
>
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.
```

### Nested blockquotes

```markdown
> Dorothy followed her through many of the beautiful rooms in her castle.
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.
```

### Blockquotes with other elements

```markdown
> #### The quarterly results look great!
> - Revenue was off the chart.
> - Profits were higher than ever.
>
> *Everything* is going according to **plan**.
```

## Lists

You can organize items into ordered and unordered lists.

### Ordered lists

```markdown
1. First item
2. Second item
3. Third item
4. Fourth item
```

The numbers don’t have to be in numerical order, but the list should start with the number `1.`.

```markdown
1. First item
1. Second item
1. Third item
1. Fourth item
```

Nested ordered list:

```markdown
1. First item
2. Second item
3. Third item
    1. Indented item
    2. Indented item
4. Fourth item
```

### Unordered lists

```markdown
- First item
- Second item
- Third item
- Fourth item
```

Nested unordered list:

```markdown
- First item
- Second item
- Third item
    - Indented item
    - Indented item
- Fourth item
```

If you need to start an unordered list item with a number followed by a period (for example `1968.`), you can use a backslash (`\`) to escape the period:

```markdown
- 1968\. A great year!
- I think 1969 was second best.
```

### Adding elements in lists

To add another element in a list while preserving the continuity of the list, indent the element four spaces (or one tab).

```markdown
* This is the first list item.
* Here's the second list item.

    I need to add another paragraph below the second list item.

* And here's the third list item.
```

Code blocks are normally indented four spaces or one tab. When they’re in a list, indent them further.

```markdown
1. Open the file.
2. Find the following code block on line 21:

        <html>
          <head>
            <title>Test</title>
          </head>

3. Update the title to match the name of your website.
```

## Code

### Inline code

```markdown
At the command prompt, type `nano`.
```

### Escaping backticks

If the word or phrase you want to denote as code includes one or more backticks, you can escape it by enclosing the word or phrase in double backticks.

```markdown
``Use `code` in your Markdown file.``
```

### Code blocks

To create code blocks, indent every line of the block by at least four spaces or one tab.

```markdown
    <html>
      <head>
      </head>
    </html>
```

## Horizontal Rules

To create a horizontal rule, use three or more asterisks (`***`), dashes (`---`), or underscores (`___`) on a line by themselves.

```markdown
***
---
___
```

For compatibility, put blank lines before and after horizontal rules.

```markdown
Try to put a blank line before...

---

...and after a horizontal rule.
```

## Links

To create a link, enclose the link text in brackets and then follow it immediately with the URL in parentheses.

```markdown
My favorite search engine is [Duck Duck Go](https://duckduckgo.com).
```

### Adding titles

```markdown
My favorite search engine is [Duck Duck Go](https://duckduckgo.com "The best search engine for privacy").
```

### URLs and email addresses

```markdown
<https://www.markdownguide.org>
<fake@example.com>
```

### Formatting links

```markdown
I love supporting the **[EFF](https://eff.org)**.
This is the *[Markdown Guide](https://www.markdownguide.org)*.
See the section on [`code`](#code).
```

### Reference-style links

```markdown
In a hole in the ground there lived a hobbit. ... it was a [hobbit-hole][1], and that means comfort.

[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"
```

## Images

```markdown
![The San Juan Mountains are beautiful](/assets/images/san-juan-mountains.jpg "San Juan Mountains")
```

### Linking images

```markdown
[![An old rock in the desert](/assets/images/shiprock.jpg "Shiprock, New Mexico by Beau Rogers")](https://www.flickr.com/photos/beaurogers/31833779864/)
```

## Escaping Characters

To display a literal character that would otherwise be used to format text in a Markdown document, add a backslash (`\`) in front of the character.

```markdown
\* Without the backslash, this would be a bullet in an unordered list.
```

## HTML

Many Markdown applications allow you to use HTML tags in Markdown-formatted text.

```markdown
This **word** is bold. This <em>word</em> is italic.
```

For security reasons, not all Markdown applications support HTML in Markdown documents. When you use block-level HTML elements like `<div>`, `<table>`, `<pre>`, and `<p>`, use blank lines to separate them from surrounding content and avoid indenting the tags.
