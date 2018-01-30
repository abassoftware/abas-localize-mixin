
<!---

This README is automatically generated from the comments in these files:
app-localize-behavior.html

Edit those files, and our readme bot will duplicate them over here!
Edit this file, and the bot will squash your changes :)

The bot does some handling of markdown. Please file a bug if it does the wrong
thing! https://github.com/PolymerLabs/tedium/issues

-->

[![Build status](https://travis-ci.org/toshovski/app-localize-behavior.svg?branch=master)](https://travis-ci.org/PolymerElements/app-localize-behavior)

## Polymer.AppLocalizeBehavior

`Polymer.AppLocalizeBehavior` wraps the [format.js](http://formatjs.io/) library to
help you internationalize your application. Note that if you're on a browser that
does not natively support the [Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)
object, you must load the polyfill yourself. An example polyfill can
be found [here](https://github.com/andyearnshaw/Intl.js/).

`Polymer.AppLocalizeBehavior` supports the same [message-syntax](http://formatjs.io/guides/message-syntax/)
of format.js, in its entirety; use the library docs as reference for the
available message formats and options.

Sample application loading resources from an external file:

```html
<dom-module id="x-app">
  <template>
    <div>[[localize('hello', 'name', 'Batman')]]</div>
  </template>
  <script>
  class XApp extends Polymer.AppLocalizeBehavior(Polymer.Element) {
    static get is() {
      return "x-app"
    }
   
   static get properties() {
     return {
       language: {
         value: 'en'
       }
     }   
   }
   
   connectedCallback() {
     super.connectedCallback();  
     this.loadResources(this.resolveUrl('locales.json'));
   }
 }
 customElements.define(XApp.is, XApp);
 &lt;/script>
 </dom-module>
```

Alternatively, you can also inline your resources inside the app itself:

```html
<dom-module id="x-app">
  <template>
    <div>[[localize('hello', 'name', 'Batman')]]</div>
  </template>
  <script>
  class XApp extends Polymer.AppLocalizeBehavior(Polymer.Element) {
    static get is() {
      return "x-app"
    }
   
   static get properties() {
     return {
       language: {
         value: 'en'
       },
       resources: {
         value: function() {
           return {
             'en': { 'hello': 'My name is {name}.' },
             'fr': { 'hello': 'Je m\'apelle {name}.' }
           }
         }
       }
     }   
   }
   
   connectedCallback() {
     super.connectedCallback();  
     this.loadResources(this.resolveUrl('locales.json'));
   }
 }
 customElements.define(XApp.is, XApp);
 &lt;/script>
 </dom-module>
```

### Fallback Language
This language is used when no translation is found for default language.

### Stuctured Files
The resouce file can also have a deep structure for instace:

```JSON
"en": {
  "welcome": {
    "polite": "Welcome Sir!",
    "normal": "Welcome!"
  }
}
```

You can access these structures by extending you translation key with :.

```HTML
<div>{{localize('welcome:polite')}}</div>
```

### Language Changed Event
Using this component in an Structured Polymer app it can get anoying to bind language property downwards.
Alternatively to oneway downwards binding you can fire an event that will be caugt by this behavior.
    
    //change language
    this.dispatchEvent(new CustomEvent('app-localize-language-changed'), {detail: {language: 'fr'}})
    //change fallbackLanguage
    this.dispatchEvent(new CustomEvent('app-localize-fallback-language-changed'), {detail: {fallbackLanguage: 'fr'}})

This repo wraps Format.js which means you should use a Language Key specific 
to a region to have everything formated for that region for instance en_US and en_UK. 
But this can get out of hands very quickly. Thats why this behavior fallsback to the language 
when region specific does not provide a result. With this you can only use region in your 
locales.json when needed:

```JSON
"en": {
  "welcome": "Welcome" 
},
"en_US": {
  "color": "color" 
}
"en_UK": {
  "color": "colour" 
}
```JavaScript
language = 'en_US';
localize("welcome");// Welcome
localize("color"); // color
language = 'en_UK';
localize("welcome"); // Welcome
localize("color"); // colour
```