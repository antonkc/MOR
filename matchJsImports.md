# Match js imports

Check [lookaround](https://www.regular-expressions.info/lookaround.html) for understanding lookarounds, it helped me.

This regular expression seeks javascript imports. It always has the following matching groups: $1 type (if type import, this group will have value) $2 default import, $3 wildcard imports, $4 destructure imports, $5 from text, $6 module-name, and $7 quotes style (either simple quotes or double).

This file shows how I built this regular expression for "ease" of modification.

## complete

```
(?<=(?:[\s\n;])|^)(?:import[\s\n]*((?:(?<=[\s\n])type)?)(?=[\n\s\*\{])[\s\n]*)((?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?(?=(?:[\n\s]*,[\n\s]*[\{\*])|(?:[\n\s]+from)))?)[\s\n,]*((?:\*[\n\s]*(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*))(?=[\n\s]+from))?)[\s\n,]*((?:\{[n\s]*(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?[\s\n]*,?[\s\n]*)*\}(?=[\n\s]*from))?)(?:[\s\n]*((?:from)?))[\s\n]*(?:["']([^"']*)(["']))[\s\n]*?;?
```

## base

```
(?<=(?:[\s\n;])|^)#start#(#default#?)[\s\n,]*(#wildImports#?)[\s\n,]*(#destructImports#?)#from#[\s\n]*#source#[\s\n]*?;?
```

### start

precompiled: ```(?:import[\s\n]*((?:(?<=[\s\n])type)?)(?=[\n\s\*\{])[\s\n]*)```

```(?:import[\s\n]*((?:(?<=[\s\n])type)?)##validAfterImport##[\s\n]*)```

#### validAfterImport

```(?=[\n\s\*\{])```

### default

precompiled: ```(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?(?=(?:[\n\s]*,[\n\s]*[\{\*])|(?:[\n\s]+from)))```

```(?:#var#(?:[\s\n]+#asSyn#)?##validAfterDefault##)```

#### validAfterDefault

precompiled: ```(?=(?:[\n\s]*,[\n\s]*[\{\*])|(?:[\n\s]+from))```

```(?=###nextBlockAfterDefault###|###fromAfterDefault###)```

##### nextBlockAfterDefault

```(?:[\n\s]*,[\n\s]*[\{\*])```

##### fromAfterDefault

```(?:[\n\s]+from)```

### destructImports

precompiled: ```(?:\{[n\s]*(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?[\s\n]*,?[\s\n]*)*\}(?=[\n\s]*from))```

```(?:\{[n\s]*##destructComponent##*\}##validAfterDestruct##)```

#### destructComponent

precompiled: ```(?:(?:[_\$\w][_\$\w0-9]*)(?:[\s\n]+(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*)))?[\s\n]*,?[\s\n]*)```

```(?:#var#(?:[\s\n]+#asSyn#)?[\s\n]*,?[\s\n]*)```

#### validAfterDestruct

```(?=[\n\s]*from)```

### wildImports

precompiled: ```(?:\*[\n\s]*(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*))(?=[\n\s]+from))```

```(?:\*[\n\s]*#asSyn###validAfterWild##)```

#### validAfterWild

```(?=[\n\s]+from)```

### from

```(?:[\s\n]*((?:from)?))```

### source

```(?:["']([^"']*)(["']))```

### asSyn

precompiled: ```(?:as[\s\n]+(?:[_\$\w][_\$\w0-9]*))```

```(?:as[\s\n]+#var#)```

### var

```(?:[_\$\w][_\$\w0-9]*)```
