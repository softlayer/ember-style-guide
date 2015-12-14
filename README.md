
# Ember.js Style Guide

This document extends the [JavaScript Style Guide](javascript.md) to provide
Ember.js specific guidance.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)


## Table of Contents

### Conventions

* [Computed Properties](#computed-properties)
* [Templates](#templates)


### Grammar

* [Type Checking](#type-checking)
* [Comments](#comments)
    * [JSDoc tags utilizing](#jsdoc-tags-utilizing)
    * [General rules](#general-rules)
    * [File Structure](#file-structure-1)
    * [Modules](#modules)
    * [Functions](#functions)
    * [Constants](#constants)
    * [Properties](#properties)
    * [Acceptable Deviations](#acceptable-deviations)
    * [Example of the tags in use](#example-of-the-tags-in-use)
* [Newlines](#newlines)


### Testing
* [Ember.js Testing Guide](ember-testing.md)


### Ember Data

* [Be explicit with Ember Data attribute types](#be-explicit-with-ember-data-attribute-types)


### File Structure

* [File Structure](#file-structure-2)


### Architecture Philosophy

* [Where should DOM interactions occur in an Ember application?](#where-should-dom-interactions-occur-in-an-ember-application)
* [Where to put actions in an Ember application](#where-to-put-actions-in-an-ember-application)
* [Routes](#routes)
* [Do not redefine the `init()` method, or do so with full knowledge of what you are doing](#do-not-redefine-the-init-method-or-do-so-with-full-knowledge-of-what-you-are-doing)
* [Do not redefine the `willInsertElement()`, `didInsertElement()`, `willClearRender()`, `willDestroyElement()`, or `didDestroyElement()` methods, or do so with full knowledge of what you are doing](#do-not-redefine-the-willinsertelement-didinsertelement-willclearrender-willdestroyelement-or-diddestroyelement-methods-or-do-so-with-full-knowledge-of-what-you-are-doing)
* [Observers](#observers)


---

### Computed Properties

* **MUST** always return a value
    * At minimum a `null` value
    * Other values are acceptable as appropriate


### Templates

* **MUST** use double quotes in all template syntax (e.g. attributes, property
values, closure actions, etc) for non-bound values
    * **MAY** use single quotes if passing a value that contains a double
    quote, such as with `{{the-component label='The "First" One'}}`, though it
    is **RECOMMENDED** to use HTML entities or bound values.


### Type checking

* **MUST** be performed via the use of `Ember.typeOf` for every type except for `Symbol`.
* `Symbol` **MUST** be checked via `typeof`, due to
[this Ember.js issue](https://github.com/emberjs/ember.js/issues/11673).


### Comments

Since JSDoc does not yet currently fully support ES6 syntax its conventions do
not always reflect the architecture of an Ember application. The style guides
and examples in this section serve to remedy these discrepancies.


##### JSDoc tags utilizing

Only the following Block Tags are acceptable for use in documenting your code:

* [@abstract](http://usejsdoc.org/tags-abstract.html)
* [@augments](http://usejsdoc.org/tags-augments.html)
* [@callback](http://www.usejsdoc.org/tags-callback.html)
* [@constant](http://www.usejsdoc.org/tags-constant.html)
* [@default](http://usejsdoc.org/tags-default.html)
    * **MUST** be used for types other than String, Number, Array, Boolean, or
    null
    * For String, Number, Array, Boolean, and null types the
    [default tag plugin](https://github.com/notmessenger/jsdoc-plugins#defaulttag)
    will automatically add them
* [@enum](http://www.usejsdoc.org/tags-enum.html)
* [@example](http://usejsdoc.org/tags-example.html)
* [@fires](http://www.usejsdoc.org/tags-fires.html)
* [@function](http://www.usejsdoc.org/tags-function.html)
    * Only set a name when contained within the "actions" hash. Then the name
    **MUST** be preceded by the text of “actions:”  See the code example in the
    "[Example of the tags in use](#example-of-the-tags-in-use)" section.
* [@ignore](http://usejsdoc.org/tags-ignore.html)
* [@listens](http://www.usejsdoc.org/tags-listens.html)
    * **MUST NOT** use for `.on()` calls - the
    [emberListensTag plugin](https://github.com/notmessenger/jsdoc-plugins#emberlistenstag)
    will automatically add them
* [@module](http://www.usejsdoc.org/tags-module.html)
* [@override](http://www.usejsdoc.org/tags-override.html)
* [@param](http://www.usejsdoc.org/tags-param.html)
* [@private](http://www.usejsdoc.org/tags-private.html)
* [@protected](http://www.usejsdoc.org/tags-protected.html)
* [@returns](http://www.usejsdoc.org/tags-returns.html)
* [@see](http://www.usejsdoc.org/tags-see.html)
* [@throws](http://www.usejsdoc.org/tags-throws.html)
* [@type](http://usejsdoc.org/tags-type.html)
* [@typedef](http://usejsdoc.org/tags-typedef.html)

Only the following Inline Tags are acceptable for use in documenting your code:

* [@link](http://www.usejsdoc.org/tags-inline-link.html)

These tags are not currently allowed for use but once JSDoc offers additional
support for them aligning with our needs we will begin using them.  In the
meantime the `@augments` tag should be used.

* [@mixes](http://usejsdoc.org/tags-mixes.html)
* [@mixin](http://usejsdoc.org/tags-mixin.html)


##### General rules

* `@type` and `@param`
    * types **MUST** be their most generic form (e.g. Array vs ember.Array,
    Object vs ember.Object)
    * a `*` refers to native JavaScript types
    * more specific ember.* types **MUST** be used when warranted

* `@override`
    * This tag indicates that a symbol overrides a symbol with the same name in
    a parent class.  Given this definition it could be argued that properties
    such as `classNameBindings`, `tagName`, and others would fall within this
    category and that `beforeModel`, `setupController`, and similar certainly do.
    Even with limited experience with Ember.js you will quickly learn to
    recognize these properties and methods and realize that they are overriding
    their parental definitions.  Therefore, to reduce the tediousness of having
    to declare this tag in all of these places then, the following rules are to
    be observed:
        * This tag **MUST NOT** be used in Route or Component objects
        * This tag **MUST** be used as appropriate in all other locations (i.e.
        Ember Data application adapter)

* It is **RECOMMENDED** to alphabetize the contents of the `@param`, `@type`,
and `@returns` tags when multiple types are represented.


##### File structure

This commenting structure **MUST** be used as a template when creating new
Component, Controller, Mixin, or Route files.  Route files will not contain all
of the represented structures. The "[File Structure](#file-structure-2)" Section
describes what should be placed in each section.

```
...({

    // -------------------------------------------------------------------------
    // Dependencies


    // -------------------------------------------------------------------------
    // Attributes


    // -------------------------------------------------------------------------
    // Actions


    // -------------------------------------------------------------------------
    // Events


    // -------------------------------------------------------------------------
    // Properties


    // -------------------------------------------------------------------------
    // Observers


    // -------------------------------------------------------------------------
    // Methods

});
```

##### Modules

* **MUST** have a DocBlock immediately preceding the definition
* A short description...
    * ... **MUST** be present if there are no other DocBlocks in the file
    * ... **MAY** be included in all other scenarios as appropriate
* **MAY** contain a long description
* **MUST** contain a `@module` tag, listed as the first one
    * **MUST NOT** be followed by a value
* **MAY** contain any of these tags:
    * @augments
    * @example
    * @see
    * @link
* **MUST NOT** contain any other tags

```javascript
import Ember from 'ember';
import TooltipEnabled from 'sl-ember-components/mixins/sl-tooltip-enabled';

/**
 * Short description
 *
 * A longer description that
 * spans multiple lines
 *
 * @module
 * @augments ember/Component
 * @augments sl-ember-components/mixins/sl-tooltip-enabled
 */
export default Ember.Component.extend( TooltipEnabled, {
    ...
});
```

##### Functions

* **MUST** have a DocBlock immediately preceding the definition
* **SHOULD** contain a short description
* **MAY** contain a long description
* **MUST** contain
    * @function
    * @override IF overriding a previously-defined function
    * @returns
* **MAY** contain any of these tags:
    * @abstract
    * @callback
    * @example
    * @listens
    * @param
    * @private
    * @protected
    * @see
    * @throws
* **MUST NOT** contain any other tags

```javascript
/*
 * @override
 * @function
 * @returns {undefined}
 */
config: function() {...},

/*
 * @function
 * @param {Number} first
 * @param {Number} second
 * @returns {Number}
 */
add: function( first, second ) {...}
```

##### Constants

* **MUST** have a DocBlock immediately preceding the definition
* **SHOULD** contain a short description
* **MAY** contain a long description
* **MUST** contain
    * @constant
* **SHOULD** contain
    * @default if a non-simple type
* **MAY** contain
    * @example
    * @link
    * @see
* **MUST NOT** contain any other tags

```javascript
/**
 * Number of months in a year
 *
 * @constant {Number}
 */
numberOfMonths: 12
```

##### Properties

* **MUST** have a DocBlock immediately preceding the definition
* **SHOULD** contain a short description
* **MAY** contain a long description
* **MUST** contain
    * @type
* **SHOULD** contain
    * @default if a non-simple type
* **MAY** contain
    * @example
    * @link
    * @private
    * @protected
    * @see
* **MUST NOT** contain any other tags

```javascript
/*
 * @override
 * @type {String}
 */
name: 'configuration',

/**
 * String representing the full timezone name, as used by and interpreted by Moment-timezone
 *
 * @type {ember/String}
 */
timezone: null,

/**
 * Emergency Notification model
 *
 * @type {?Array.<module:app/models/emergency-notification>}
 */
emergencyNotifications: null
```

##### Acceptable Deviations

To lessen the burden of creating documentation in scenarios where it does not
add any value, it is not required to create full DocBlocks in the following
scenarios:

* Within...
    * Ember Components
    * Ember Controllers
* For...
    * dependencies
    * attributes
    * actions hash (NOT the individual actions!)

In these scenarios a shortened, single-line comment containing the `@type` tag
is all that is needed.

```javascript
import Ember from 'ember';
import TooltipEnabled from 'sl-ember-components/mixins/sl-tooltip-enabled';

/**
* @module
* @augments ember/Component
* @augments sl-ember-components/mixins/sl-tooltip-enabled
*/
export default Ember.Component.extend( TooltipEnabled, {

    // -------------------------------------------------------------------------
    // Dependencies

    // -------------------------------------------------------------------------
    // Attributes

    /** @type {String[]} */
    classNames: [
        'alert',
        'sl-alert'
    ],

    /** @type {String[]} */
    classNameBindings: [
        'themeClassName',
        'dismissable:alert-dismissable'
    ],

    /** @type {String} */
    ariaRole: 'alert',

    // -------------------------------------------------------------------------
    // Actions

    /** @type {Object} */
    actions: {

        /**
         * Trigger a bound "dismiss" action when the alert is dismissed
         *
         * @function actions:dismiss
         * @returns {undefined}
         */
        dismiss: function() {
            this.sendAction( 'dismiss' );
        }
    },

    // -------------------------------------------------------------------------
    // Events

    // -------------------------------------------------------------------------
    // Properties

    // -------------------------------------------------------------------------
    // Observers

    // -------------------------------------------------------------------------
    // Methods

});
```

##### Example of the tags in use

```javascript
import Ember from 'ember';
import TooltipEnabled from 'sl-ember-components/mixins/sl-tooltip-enabled';

/**
 * The provided command line arguments
 *
 * @ignore
 * @type {Array.<String, *>}
 */
var argv = require( 'minimist' )( process.argv.slice( 2 ) );

/**
 * @module
 * @augments ember/Component
 * @augments module:app/mixins/the-mixin
 * @augments sl-ember-components/mixins/sl-tooltip-enabled
 */
export default Ember.Component.extend( TooltipEnabled, {

    // -------------------------------------------------------------------------
    // Dependencies

    // -------------------------------------------------------------------------
    // Attributes

    /** @type {String[]} */
    classNames: [
        'alert'
    ],

    // -------------------------------------------------------------------------
    // Actions

    /** @type {Object} */
    actions: {

        /**
         * Trigger a bound "dismiss" action when the alert is dismissed
         *
         * @function actions:dismiss
         * @returns {undefined}
         */
        dismiss: function() {
            this.sendAction( 'dismiss' );
        }
    },

    // -------------------------------------------------------------------------
    // Events

    // -------------------------------------------------------------------------
    // Properties

    /**
     * Access level
     *
     * @constant
     */
    accessLevel = 1,

    /**
     * @typedef {Object.<String, Boolean>} ActionDivider
     * @property {Boolean} divider Must be set to true
     */

    /**
     * @typedef {Object} ActionOption
     * @property {String} action
     * @property {String} label
     */

    /**
     * Definition for the Actions button
     *
     * @example
     *  [{ divider: true },
     *  { action: 'cancelItem', label: 'Cancel Device' }]
     *
     * @type {Array.<ActionDivider|ActionOption>}
     */
    actionsButton: [
        { divider: true },
        { action: 'cancelItem', label: 'Cancel Device' }
    ],

    /**
     * Whether to make the alert dismissible or not
     *
     * @type {Boolean}
     */
    dismissible: false,

    /**
     * Array of filter objects
     *
     * @type {ember/Array}
     * @default ember/Array
     */
    filters: Ember.A(),

    /**
     * Alias for application loading flag
     *
     * @type {module:app/controllers/application~isLoading}
     */
    isLoading: Ember.computed.alias( 'controllers.application.isLoading' ),

    // -------------------------------------------------------------------------
    // Observers

    /**
     * Initialize children array
     *
     * @function
     * @returns {undefined}
     */
    initChildren: Ember.on(
        'init',
        function() {
            this.set( 'children', [] );
        }
    ),

    /**
     * React to route changes
     *
     * @function
     * @returns {undefined}
     */
    reactToRouteChange: Ember.observer(
        'currentRouteName',
        function() {
            ...
        }
    ),

    // -------------------------------------------------------------------------
    // Methods

    /**
     * Example callback definition
     *
     * @callback thisIsACallback
     * @param {Number} responseCode
     * @param {String} responseMessage
     */

    /**
     * Use the callback
     *
     * @function
     * @param {thisIsACallback} callback
     * @returns {undefined}
     */
    usingTheCallback: function( callback ) {
        ...
    },

    /**
     * Returns lowercase path when ember-cli-mirage is not enabled
     *
     * @override
     * @param {String} type
     * @returns {String}
     */
    pathForType: function( type ) {
        type = this._super( ...arguments );
        return config.isEmberCliMirageEnabled ? type : type.toLowerCase();
    },

    /**
     * The generated Bootstrap "theme" style class for the alert
     *
     * @function
     * @returns {String} Defaults to "alert-info"
     */
    themeClassName: function() {
        return 'alert-' + this.get( 'theme' );
    }.property( 'theme' ),

    /**
     * Does some secret-squirrel stuff
     *
     * Also refer to {@link module:app/components/info-button} for more details
     *
     * @private
     * @function
     * @param {ember/Array} input
     * @param {String} [modifier]
     * @throws {ember.assert} If `input` is empty
     * @returns {ember/RSVP.Promise} Resolution of promise contains possible types of String, Number, Boolean
     */
    secretStuff: function( input, modifier ) {
        Ember.assert(
            '`input` must not be empty',
            !Ember.isEmpty( input )
        );

        return Ember.RSVP.Promise();
    },

    /**
     * @abstract
     * @function
     * @returns {undefined}
     */
    showHandler() {}
});

/**
 * @memberof module:addon/components/sl-grid
 * @enum {String}
 * @property {String} LEFT "left"
 * @property {String} RIGHT "right"
 */
export const ColumnAlign = Object.freeze({
    LEFT: 'left',
    RIGHT: 'right'
});
```


### Newlines

* Newlines **MUST** be employed in the following scenarios:
    * When defining:
        * attributeBindings
        * classNames
        * classNameBindings
    * When using:
        * Ember.observer
        * Ember.on
        * Ember.computed
        * Ember.assert
        * In assert statements in tests

```javascript
// SAMPLE: attributeBindings, classNames, classNameBindings

attributeBindings: [
    'data-target',
    'data-toggle',
    'disabled',
    'type'
],

classNames: [
    'form-group'
],

classNameBindings: [
    'themeClassName',
    'dismissable:alert-dismissable'
]


// SAMPLE: Ember.observer, Ember.on, Ember.computed

updateData: Ember.observer(
    'series',
    function() {
        ...
    }
),

setupChart: Ember.on(
    'didInsertElement',
    function() {
        ...
    }
),

themeClassName: Ember.computed(
    'theme',
    'anotherTheme',
    function() {
        ...
    }
)

setupTypeahead: Ember.computed(
    'suggestions',
    Ember.on(
        'didInsertElement',
        function() {
            ...
        }
    )
)


// SAMPLE: Test assertions

assert.strictEqual(
    component.get( 'dismissable' ),
    true,
    'Component is dismissable'
);
```

* The requirement to use soft tabs set to 4 spaces is intended to be applied
to JavaScript files but should also be applied to JSON files whenever possible.
A scenario where this is not possible though, for example, is in the
`package.json` file. Whenever the `-save` or `--save-dev` flags are used the
file is re-created with 2 spaces so there is no need to try to maintain 4 spaces.



### Be explicit with Ember Data attribute types

Even though Ember Data can be used without using explicit types in `attr()`,
always supply an attribute type to ensure the right data transform is used

```javascript
// Good
export default DS.Model.extend({
    firstName: DS.attr( 'string' ),
    jerseyNumber: DS.attr( 'number' )
});


// Bad
export default DS.Model.extend({
    firstName: DS.attr(),
    jerseyNumber: DS.attr()
});
```


### File Structure
The contents in a file **MUST** be organized in the following order for each
type that exists:

* Dependencies
    * services
* Attributes
    * arrangedContent
    * attributeBindings
    * classNames
    * classNameBindings
    * defaultLayout / layout / layoutName
    * queryParams
    * sortBy
    * tagName
    * template / templateName
* Actions
* Events
* Properties
* Observers
* Methods

Within each of the represented structures above the attributes, action,
properties, etc **MUST** be listed alphabetically in their respective sections.


### Where should DOM interactions occur in an Ember application?

* **SHOULD** be contained in the Component whenever possible.  This is the
closest layer to where the DOM was likely created.
* If the interaction needs to be shared across Components or if there is an
application-specific implementation required, the routes **SHOULD** be used.


### Where to put actions in an Ember application

* As close to where things are "happening".  This is most usually the Component.
* If the action needs to be shared with other Components or Routes, place it in
the lowest-level of shared access between the items requiring shared access.

### Routes

* Manage application state
* Can contain application-specific DOM knowledge and interaction, though
Components should be the first place such needs should be implemented


### Do not redefine the `init()` method, or do so with full knowledge of what you are doing

* Instead use the `Ember.on( 'init', function() {} )` pattern
* [http://reefpoints.dockyard.com/2014/04/28/dont-override-init.html](http://reefpoints.dockyard.com/2014/04/28/dont-override-init.html)
* [https://github.com/emberjs/ember.js/issues/3399](https://github.com/emberjs/ember.js/issues/3399)
* Having said not to use it
[http://reefpoints.dockyard.com/2014/04/17/ember-object-self-troll.html](http://reefpoints.dockyard.com/2014/04/17/ember-object-self-troll.html)
offers a reason for when you should
    * If you do you will likely need to call `this._super( ...arguments )`,
    though this will depend on your exact needs.
    * It is the last part of the article about subclasses being able to
    override that should be the take away from this article


### Do not redefine the `willInsertElement()`, `didInsertElement()`, `willClearRender()`, `willDestroyElement()`, or `didDestroyElement()` methods, or do so with full knowledge of what you are doing

* Instead use the `Ember.on( <eventName>, function() {} )` pattern
* Rarely will you need to redefine any of these methods.  If you do you will
likely need to call `this._super( ...arguments )`, though this will
depend on your exact needs.

### Observers

A good pattern to follow for improved performance is the one presented in the
video at [https://youtu.be/cp1Jk92ve2s?t=1097](https://youtu.be/cp1Jk92ve2s?t=1097)
from timestamp 18:18 to 20:15
