---
layout: post
title: "Azure Resource Manager Template tricks"
description: "A set of tricks I'm using to make ARM templates"
category: 
tags: []
---

Azure Resource Manager Templates are a nice way to express your infrastructure as code when developing in the Azure cloud. The template language is a bit *interesting* to say the least though so you might need some tricks to bend it to your will.

## Variable tricks

I'll start with a few building blocks.

### Using a string as a toggle

Sometimes you might need to turn a string into a toggle depending on if it has a value or not. The following construct turns an empty string into a `0` and any other value of string into a `1`.

```javascript
length(replace(take(string(length(variables('input'))),1),'0',''))
```

Let's walk though it from the inside out.

1. we start with the length of the string, `length(variables('input'))`
1. convert it to string and take the first character, `take(string(...),1)`, this will be `0` only if length is zero, otherwise `1...9`
1. we replace `"0"` with `""`, yielding a string of length either `0` or `1`
1. finally we convert this into an `int` either `0` or `1` depending on if the input was an empty string or not

### Using a boolean as a toggle

We can use a similar trick to use a boolean as a toggle.

```javascript
length(replace(take(string(variables('input')),1),'f',''))
```

1. start by converting the boolean into a string, `string(variables('input'))` yielding either `"true"` or `"false"`
1. take the first character and replace `"f"` with `""`, `replace(take(...,1),'f','')`, yielding a string of length `1` or `0`
1. the length of the string `length(...)` can directly be converted into a toggle

### Boolean operations on toggles

We can combine the toggles using `AND` or `XOR` using the math functions,

```javascript
mul(variables('a'),variables('b')) // AND
mod(add(variables('a'),variables('b'),2) //XOR
```

using some more string processing we can also achieve an `OR` operation

```javascript
length(replace(take(string(add(variables('a'),variables('b))),1),'0',''))
```

## Nested deployments tricks

Because nested deployments are processed twice from a templating perspective we can use that to conditionally deploy resources and also do some other tricks.

### Toggling resource sets

Using the toggles and nested deployments we can enable and disable resources conditionally depending on the input.

```json 
{
  "$schema": "...",
  "contentVersion": "...",
  "parameters": {
    "yesNo": {
      "type": "bool",
      "defaultValue": true
    }
  },
  "variables": {
    "yesNoToggle": "length(replace(take(string(parameters('yesNo')),1),'f',''))",
    "yesNoResources": [
      [],
      [{ ... }]
    ]
  },
  "resources": [{
    "name": "...",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2016-09-01",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "...",
        "contentVersion": "...",
        "resources": "[variables('yesNoResources)[variables('yesNoResourceToggle')]]"
      }
    }
  }]
}
```

### Using `reference()` in variables

Normally the `reference()` function cannot be used outside the `resources` part of the template since it's evaluated at runtime after variables have be computed. Using nested deployments however we can pass the returned value of the function as parameters to the nested template.

```json
{
  "$schema": "...",
  "contentVersion": "...",
  "resources": [{
    "name": "...",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2016-09-01",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "...",
        "contentVersion": "...",
        "parameters": {
          "innerValue": {
            "type": "string"
          }
        },
        "resources": [ 
          {
            "outputs": {
              "output": {
                "type": "string",
                "value": "[[parameters('innerValue')]"
              }
            }
          }
        ]
      },
      "parameters": {
        "innerValue": {
          "value": "[reference(...)]"
        }
      }
    }
  }]
}
```

Note that references to parameters defined in the inner template must be escaped as `[[` because otherwise it would refer to a parameter in the outer template that is not defined.