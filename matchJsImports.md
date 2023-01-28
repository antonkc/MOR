# Match js imports

Check [lookaround](https://www.regular-expressions.info/lookaround.html) for understanding lookarounds, it helped me.

This regular expression seeks javascript imports. It always has the following matching groups: $1 Default member import, $2 wildcardImport, $3 member imports by key, $4 from text, $5 module-name, and $6 quotes used (either simple quotes or double).

This file shows how I build this regular expression for "ease" of modification.

## complete

```
(?<=(?:[\s\n;])|^)import[\s\n]+?((?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+as[\s\n]+(?:[_\$\w][_\$\w0-9]*))?)?)(?:(?:[\s\n]*,?(?:(?<=import[\s\n]+)|(?<=,))[\s\n]*)|(?:[\s\n]*(?=from)))((?:\*(?:[\s\n]+as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?)(?:(?:[\s\n]*,?(?:(?<=import[\s\n]+)|(?<=,))[\s\n]*)|(?:[\s\n]*(?=from)))((?:\{[\s\n]*(?:(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+as[\s\n]+(?:[_\$\w][_\$\w0-9]*))?)[\s\n]*,?[\s\n]*)*\})?)(?:(?<=[\{\}\[\]\(\),])|(?<=[\s\n]+))\s*?((?:from)?)\s*?(?:["']([^"']*)(["']))\s*?;?
```

## base

```
(?<=(?:[\s\n;])|^)import[\s\n]+?(#mainImport#?)#importsSeparator#(#wildImports#?)#importsSeparator#(#keyedImports#?)#isAfterSpaceOrSymbol#\s*?((?:from)?)\s*?#source#\s*?;?
```

### wildImports

```(?:\*#asSyntax#)```

### keyedImports

precompiled: ```(?:\{[\s\n]*(?:(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+as[\s\n]+(?:[_\$\w][_\$\w0-9]*))?)[\s\n]*,?[\s\n]*)*\})```

```(?:\{[\s\n]*(?:#mainImport#[\s\n]*,?[\s\n]*)*\})```

### source

```(?:["']([^"']*)(["']))```

## global uses

### mainImport

```(?:#var##asSyntax#?)```

### isAfterSpaceOrSymbol

precompiled: ```(?:(?<=[\{\}\[\]\(\),])|(?<=[\s\n]+))```

```(?:#isAfterSymbol#|#isAfterSpace#)```

### isAfterSpace

```(?<=[\s\n]+)```

### isAfterSymbol

```(?<=[\{\}\[\]\(\),])```

### importsSeparator

```(?:(?:[\s\n]*,?(?:(?<=import[\s\n]+)|(?<=,))[\s\n]*)|(?:[\s\n]*(?=from)))```

### asSyntax

```(?:[\s\n]+as[\s\n]+#var#)```

### var

```(?:[_\$\w][_\$\w0-9]*)```
