# Creating Markup
Because Field Markup follows the Expert field markup provided by Display Suite, it provides the same structure:

```json
"field-name" : {
  "outer-wrapper" : {
  },
  "title-wrapper" : {
  },
  "inner-wrapper" : {
  },
  "item-wrapper" : {
  }
}
```
Where:

```field-name``` is the machine name of the field you are targeting

```outer-wrapper``` is the markup that surrounds the entire field, including the field title (if shown)

```title-wrapper``` is the markup that surrounds just the fields title (if shown)

```inner-wrapper``` is the markup that will wrap the field contents, but outside any multi-value fields

```item-wrapper``` is the markup that will wrap each value in a multi-value field or the contents of a single value field.

So a simple structure could look something like this:

```json
"field-name" : {
  "outer-wrapper" : {
    "markup" : "div"
  },
  "title-wrapper" : {
    "markup" : "h1"
  },
  "inner-wrapper" : {
    "markup" : "ul"
  },
  "item-wrapper" : {
    "markup" : "li"
  }
}
```

Which would generate markup like this:

```html
<div>
  <h1>Field title</h1>
  <ul>
    <li>First field value</li>
    <li>Second field value</li>
  </ul>
</div>
```

# IDs, Classes, and Attributes
Simply adding HTML markup wouldn't be too useful if it wasn't possible to add IDs, classes, or attributes.  Fortunately it's possible to do so by adding specific keys to the JSON structure:

```json
"field-name" : {
  "outer-wrapper" : {
    "markup"  : "div",
    "id"      : "field-id",
    "class" : "first-class second-class"
  },
  "title-wrapper" : {
    "markup"  : "h1",
    "class" : "field-title"
  },
  "inner-wrapper" : {
    "markup" : "ul"
  },
  "item-wrapper" : {
    "markup"     : "li",
    "attributes" : "data-attribute=\"value\""
  }
}
```
Which would generate markup like this:

```html
<div id="field-id" class="first-class second-class">
  <h1 class="field-title">Field title</h1>
  <ul>
    <li data-attribute="value">First field value</li>
    <li data-attribute="value">Second field value</li>
  </ul>
</div>
```

Note the `\"` structure in the `item-wrapper`.  This allows quotations to be embedded inside JSON strings.  While there are a number of available keys, those relating to markup are:

**`id`** - The value will be used as the ID of the field.  Using `"id" : "any-id"` results in `id="any-id"` being inserted into the markup.

**`class`** - The value will be used as the classes for the field.  Multiple classes must be separated by a space, just like HTML markup.  This value will be automatically converted to an array if the field formatting requires that format.  Using `"class" : "class-one class-two"` results in `class="class-one class-two"` being inserted into the markup.

**`attributes`** - The value will be inserted into the markup without any additional processing.  Note that double quotations must be escaped using `\"` to form valid HTML.  Using `"attributes" : "data-show-value=\"variable value\""` will insert `data-show-value="variable value"` into the markup.  The `attribute` key can be used to insert any HTML attribute or combination of attributes into the markup.  While not recommended (inline styles are generally discouraged), `"attribute" : "style=\"color:red;\" target=\"_blank\""` is a valid value and would result in `style="color:red;" target="_blank"` being inserted into the markup.

## Using placeholders

The markup discussed so far is hard coded, in the sense that the output will always be the same.  It is possible to provide field-specific values using placeholder codes. The two most common are `#fieldid#` and `#merge#`.

So giving a JSON structure like this:

```json
"field-name" : {
  "outer-wrapper" : {
    "markup" : "div",
    "id"     : "#fieldid#",
    "class"  : "#merge#"
  },
  "title-wrapper" : {
    "markup" : "h1"
  },
  "inner-wrapper" : {
    "markup" : "ul"
  },
  "item-wrapper" : {
    "markup" : "li"
  }
}
```
Would generate markup like this:

```html
<div id="field-name" class="field-class another-field-class">
  <h1>Field title</h1>
  <ul>
    <li>First field value</li>
    <li>Second field value</li>
  </ul>
</div>
```
Assuming that the machine name of the field being displayed was `field-name` and that other Drupal modules had added classes of `field-class` and `another-field-class`.

There are a number of other placeholders available, get more information on the  [Using Placeholders](https://github.com/martin-wessel-topfloor/Field-Markup/wiki/Using-Placeholders) page.

# Overriding output (control values)

The Field Markup module provides two values for the `markup` key that control markup: `#none#` and `#hide#`.  `#none#` is a general purpose value that suppresses markup generation while leaving content, while `#hide#` applies only to the `wrapper-title` `markup` and removes both the markup and the title text.  Given the following JSON: 

```json
"field-name" : {
  "outer-wrapper" : {
    "markup" : "#none#"
  },
  "title-wrapper" : {
    "markup" : "#hide#"
  },
  "inner-wrapper" : {
    "markup" : "#none"
  },
  "item-wrapper" : {
    "markup" : "p"
  }
}
```

This markup would be generated:

```html
<p>First field value</p>
<p>Second field value</p>
```

Learn more about using control values on the [Using Control Values](https://github.com/martin-wessel-topfloor/Field-Markup/wiki/Using-Control-Values) page.

# Using Templates

The `template` key will take the field definition given in the value and use it as the base JSON for the current field.  A template can be anything from a full definition including all wrappers to just a single wrapper, and a specific field definition can override any theme settings.

So a simple template  could look something like this:

```json
"#unordered-list#" : {
  "outer-wrapper" : {
    "markup" : "div"
  },
  "title-wrapper" : {
    "markup" : "h1"
  },
  "inner-wrapper" : {
    "markup" : "ul"
  },
  "item-wrapper" : {
    "markup" : "li"
  }
},

"field-name" {
  "template" : "#unordered-list#"
}
```

Which would generate markup like this:

```html
<div>
  <h1>Field title</h1>
  <ul>
    <li>First field value</li>
    <li>Second field value</li>
  </ul>
</div>
```

Learn more about using Templates on the [Using Templates](https://github.com/martin-wessel-topfloor/Field-Markup/wiki/Using-Templates) page.

# Targeting specific field instances

# Complex markup structures

# Adding content

# Altering field content

Be sure to introduce targets, placeholders, control values but use the other wiki pages to go into greater detail.