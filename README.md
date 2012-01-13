# Hypher

A small and fast JavaScript hyphenation engine compatible with Hyphenator.js language objects. Can be used in Node.js and as a jQuery plugin.

## Node.js
Hypher can be installed from NPM:

    npm install hypher hyphenation.en-us

You can then use it in your program by creating an instance of `Hypher` and giving it a language object and (optionally) an options object:

    var Hypher = require('hypher'),
        english = require('hyphenation.en-us'),
        h = new Hypher(english);

    // returns ['hy', 'phen', 'ation']
    h.hyphenate('hyphenation');

See `examples/node/` for a full example on how to use Hypher. The `hyphenate` method does not support hyphenated compound words. These should be split into individual words before being passed to the hyphenation engine and reassembled afterwards by the caller. You can also use the `hyphenateText` method to hyphenate a string of text. The `hyphenateText` method *does* support compound words and returns a string with inserted soft hyphens (`\u00AD`.)

    // returns 'Hy|phen|ation is use|ful when cen|ter jus|ti|fy|ing a text.' where `|` is a soft hyphen
    h.hyphenateText('Hyphenation is useful when center justifying a text.');

Note that an instance of the `Hypher` class should only be created once for each language object.

The language object should contain:

    {
      // The minimum number of unhyphenated characters at the left of each word. (required)
      leftmin: <number>, 

      // The minimum number of unhyphenated characters at the right of each word. (required)
      rightmin: <number>,

      // A comma separated list of hyphenation exceptions. Custom hyphenations
      // can be specified using '=' or '-' as hyphenation character. (Optional)
      exceptions: <string>,

      // A patterns object (required)
      patterns: {}
    }

Language objects are identical to the format used by [Hyphenator.js](http://code.google.com/p/hyphenator/). The only difference is how they are used. Hypher requires you to manually pass a language object, whereas Hyphenator.js automatically "registers" a language object. Language objects can be found in the [patterns repository](https://github.com/bramstein/hyphenation-patterns).

The options object may be `null` or contain:

    {
      // The minimum length of a word to be considered for hyphenation. (Optional, defaults to 4)
      minLength: <number>,

      // The any character used in the pattern file (Optional, defaults to "_". Note that Hyphenator.js
      // uses "_", but many other pattern files use ".".)
      anyChar: <string>
    }

##jQuery

To use the jQuery plugin include `dist/jquery.hypher.js` in your HTML document together with any number of language pattern files from the `dist/browser` directory in the [patterns repository](https://github.com/bramstein/hyphenation-patterns). It is important that you include `jquery.hypher.js` before any language pattern files.

    <script src="jquery.hypher.js"></script>
    <script src="en-us.js"></script>

This will extend jQuery with a `hyphenate` method. Given the following HTML:

    <p>Hyphenation is <em>important</em></p>

You can hyphenate the text content of the `p` element like so:

    $('p').hyphenate('en-us');

The `hyphenate` method only works on the text content of the elements it is called on, so in the above example the word "important" will not be hyphenated. To also include the text content of the `em` element, simply include it in your selector:

    $('p, em').hyphenate('en-us');

This naturally also applies to your own classes:

    $('p.hyphenate, em, a').hyphenate('en-us');

This will hyphenate only `p` with class `hyphenate` and `em` and `a` elements.

## License
Hypher is licensed under the three clause BSD license (see BSD.txt.)

## See also
* [Hyphenation patterns for use with Hypher](https://github.com/bramstein/hyphenation-patterns)
* [Hyphenator.js](http://code.google.com/p/hyphenator/)
