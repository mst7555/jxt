# jxt
**JSON/XML Translation for the Browser**

[![Build Status](https://travis-ci.org/legastero/jxt.png)](https://travis-ci.org/legastero/jxt)
[![Dependency Status](https://david-dm.org/legastero/jxt.png)](https://david-dm.org/legastero/jxt)
[![devDependency Status](https://david-dm.org/legastero/jxt/dev-status.png)](https://david-dm.org/legastero/jxt#info=devDependencies)

[![Browser Support](https://ci.testling.com/legastero/jxt.png)](https://ci.testling.com/legastero/jxt)

## What is this?

A basic framework for translating XML to dev-friendly JSON and back again, and can be used
both in the browser and in node.

## Installing

```sh
$ npm install jxt
```

## Building bundled/minified version (for AMD, etc)

```sh
$ grunt
```

The bundled and minified files will be in the generated `build` directory.

## How to use it

First, we define the mapping between our XML and desired JSON:

```js
var jxt = require('jxt');

var Message = jxt.define({
    name: 'message',
    namespace: 'jabber:client',
    element: 'message',
    fields: {
        to: jxt.attribute('to'),
        from: jxt.attribute('from'),
        subject: jxt.subText('jabber:client', 'subject'),
        body: jxt.subText('jabber:client', 'body')
    }
});
```

Now, we can create `Message` objects, and set fields and treat it just like JSON, and it will map it to XML.

```js
var msg = new Message();
msg.to = 'foo@example.com';
msg.body = 'giving a demo of jxt';

console.log(msg.toJSON());
console.log(msg.toString());

// {to: 'foo@example.com', body: 'giving a demo of jxt'}
// <message xmlns="jabber:client" to="foo@example.com"><body>giving a demo of jxt</body></message>
```

Mappings can be extended:

```js
var Ext = jxt.define({
    name: 'demoExt',
    namespace: 'jxt',
    element: 'demo',
    fields: {
        text: jxt.text()
    }
});

jxt.extend(Message, Ext);

var msg = new Message();
msg.demoExt.text = 'an extension';

console.log(msg.toJSON());
console.log(msg.toString());
// {demoExt: {text: 'an extension'}}
// <message xmlns="jabber:client"><demo xmlns="jxt">an extension</demo></message>
```

## Predefined Field Types

- attribute
- b64Text
- boolAttribute
- boolSub
- boolSubAttribute
- dateAttribute
- dateSub
- dateSubAttribute
- langAttribute
- multiSubText
- numberAttribute
- numberSub
- numberSubAttribute
- subAttribute
- subLangText
- subText
- text

## License

MIT

## Created By

If you like this, follow [@lancestout](http://twitter.com/lancestout) on twitter.
