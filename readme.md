# TecladoJS

TecladoJS es un fork de la librería KeyboardJS para manejar eventos de teclado. Realmente la única modificación después del fork ha sido la traducción al castellano del nombre de la librería y de algunos métodos, para su utilización en clases de programación para niños.

Si buscas una librería con intención de usarla en un proyecto profesional mi recomendación es que utilices la original:
[http://robertwhurst.github.com/KeyboardJS/](http://robertwhurst.github.com/KeyboardJS/)

Y si te ha gustado le puedes echar una mano a su autor Robert Whurst:

[![Endorse](http://api.coderwall.com/robertwhurst/endorsecount.png)](http://coderwall.com/robertwhurst)
[![Flattr This](http://api.flattr.com/button/flattr-badge-large.png)](http://flattr.com/thing/1381991)

Funcionalidades:

+ Combinaciones de teclas avanzadas.
+ Previene superposición de eventos en combinaciones. Si una combinación es muy larga, previene que se dispare el evento con combinaciones anteriores.
+ Teclas virtuales (combos).
+ Localización del teclado (de momento viene colo con teclado de USA como el original, cuando pueda creare los combos para el español)

## Ejemplos

### Asignando eventos a teclas

```javascript
teclado.cuando('a', function() {
    console.log('¡Has pulsado a!');
});

*** Al pulsar 'a'
>>> '¡Has pulsado a!'
*** Al soltar 'a'

```

### Combinación de teclas

```javascript
teclado.cuando('ctrl + m', function() {
    console.log('ctrl m!');
});

// el usuario puede pulsar las teclas en cualquier orden
*** Al pulsar 'ctrl' y 'm'
>>> 'ctrl m!'
*** Al soltar 'ctrl' y 'm'
```
### Combinación de teclas ordenada

```javascript
teclado.cuando('ctrl > m', function() {
    console.log('ctrl m!');
});

*** Al pulsar 'ctrl'
*** Al pulsar 'm'
>>> 'ctrl m!'
*** Al soltar 'ctrl' y 'm'

//si las teclas se pulsan en distinto orden no funcionará
*** Al pulsar 'm'
*** Al pulsar 'ctrl'

*** Al soltar 'm' y 'ctrl'
```

### Prevención de solapamiento

```javascript
teclado.cuando('ctrl > m', function() {
    console.log('ctrl m!');
});
teclado.cuando('shift + ctrl > m', function() {
    console.log('shift ctrl m!');
});

*** Al pulsar 'ctrl'
*** Al pulsar 'm'
>>> 'ctrl m!'
*** Al soltar 'ctrl' y 'm'

// mientras se pulsa shift ctrl m no se dispara ctrl m
*** Al pulsar 'shift' y 'ctrl'
*** Al pulsar 'm'
>>> 'shift ctrl m!'
*** Al soltar 'shift', 'ctrl' y 'm'
```

Métodos
-------



### teclado.cuando

###### Modo de uso

    teclado.cuando(string keyCombo[, function onDownCallback[, function onUpCallback]]) => object binding

Binds any key or key combo. See 'keyCombo' definition below for details. The onDownCallback is fired once the key or key combo becomes active. The onUpCallback is fired when the combo no longer active (a single key is released).

Both the onDownCallback y the onUpCallback are passed three arguments. The first is the key event, the second is the keys pressed, y the third is the key combo string.

###### Returned
Returns an object containing the following methods.

* limpiar() - Removes the key or key combo binding.
* cuando() - Allows you to bind to the keyup y keydown event of the given combo. An alternative to adding the onDownCallback y onUpCallback.



### teclado.activeKeys

###### Usage

    teclado.activeKeys() => array activeKeys

Returns an array of active keys by name.

### teclado.limpiar

###### Usage

    teclado.limpiar(string keyCombo)

Removes all bindings with the given key combo. See 'keyCombo' definition for more details.

Please note that if you are just trying to remove a single binding should use the clear method in the object returned by teclado.on instead of this. This function is for removing all binding that use a certain key.



### teclado.limpiar.key

###### Usage

    teclado.limpiar.key(string keyCombo)

Removes all bindings that use the given key.

### teclado.locale

###### Usage

    teclado.locale([string localeName]) => string localeName

Changes the locale teclado uses to map key presses. Out of the box teclado only supports US keyboards, however it is possible to add additional locales via teclado.locale.register().



### teclado.locale.register

###### Usage

    teclado.locale.register(string localeName, object localeDefinition)

Adds new locale definitions to teclado.



### teclado.macro

###### Usage

    teclado.macro(string keyCombo, array keyNames)

Accepts a key combo y an array of key names to inject once the key combo is satisfied.



### teclado.macro.remove

###### Usage

    teclado.macro.remove(string keyCombo)

Accepts a key combo y clears any y all macros bound to that key combo.



### teclado.key.name

###### Usage

    teclado.key.name(number keyCode) => array keyNames

Accepts a key code y returns the key names defined by the current locale.



### teclado.key.code

###### Usage

    teclado.key.code(string keyName) => number keyCode

Accepts a key name y returns the key code defined by the current locale.



### teclado.combo.parse

###### Usage

    teclado.combo.parse(string keyCombo) => array keyComboArray

Parses a key combo string into a 3 dimensional array.

- Level 1 - sub combos.
- Level 2 - combo stages. A stage is a set of key name pairs that must be satisfied in the order they are defined.
- Level 3 - each key name to the stage.



### teclado.combo.stringify

###### Usage

    teclado.combo.stringify(array keyComboArray) => string KeyCombo

Stringifys a parsed key combo.



Definitions
-----------

### keyCombo

A string containing key names separated by whitespace, `>`, `+`, y `,`.

###### examples

* 'a' - binds to the 'a' key. Pressing 'a' will match this keyCombo.
* 'a, b' - binds to the 'a' y 'b' keys. Pressing either of these keys will match this keyCombo.
* 'a + b' - binds to the 'a' y 'b' keys. Pressing both of these keys will match this keyCombo.
* 'a + b, c + d' - binds to the 'a', 'b', 'c' y 'd' keys. Pressing either the 'a' key y the 'b' key,
or the 'c' y the 'd' key will match this keyCombo.

###localeDefinitions

An object that maps keyNames to their keycode y stores locale specific macros. Used for mapping keys on different locales.

###### example

    {
        "map": {
            "65": ["a"],
            "66": ["b"],
            ...
        },
        "macros": [
            ["shift + `", ["tilde", "~"]],
            ["shift + 1", ["exclamation", "!"]],
            ...
        ]
    }

Language Support
----------------

teclado can support any locale, however out of the box it just comes with the US locale (for now..,). Adding a new
locale is easy. Map your keyboard to an object y pass it to teclado.locale.register('myLocale', {/*localeDefinition*/}) then call
teclado.locale('myLocale').

If you create a new locale please consider sending me a pull request or submit it to the
[issue tracker](http://github.com/RobertWHurst/teclado/issues) so I can add it to the library.

Credits
-------

I made this to enable better access to key controls in my applications. I'd like to share
it with fellow devs. Feel free to fork this project y make your own changes.
