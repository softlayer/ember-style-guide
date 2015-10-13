
# Ember.js Testing Guide

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)


## Table of Contents


### Assertions

* [assert.expect()](#assertexpect)
* [Specificity](#specificity)


### Components

* [Unit Tests](#component-unit-tests)
* [Integration Tests](#component-integration-tests)


### Rendering Templates

* [Unit and Integration Tests](#rendering-templates-1)


### Examples

* [Testing dependent keys](#example-testing-dependent-keys)
* [Testing mixins are mixed in](#example-testing-mixins-are-mixed-in)
* [Rendering templates](#example-rendering-templates)
* [Asynchronous testing](#example-asynchronous-testing)

---

### assert.expect()

* assert.expect() **MUST** only be used in asynchronous tests


### Specificity

* Assertions **MUST** be the most specific possible
    * in the case of `ember-cli-qunit` this means that `assert.strictEqual()`
    is preferred over `assert.equal()`, for example


### Component Unit Tests

* **MUST** test:
    * default values of all properties, including `Enums`, except for:
        * `attributeBindings`
        * `classNames`
        * `classNameBindings`
    * that any expected mixins are mixed in
    [See Example](#example-testing-mixins-are-mixed-in)
    * the dependent keys computed properties are observing, via the
    `_dependentKeys` property [See Example](#example-testing-dependent-keys)
    * the logic of the computed property functions
    * the logic of any actions
    * the logic of any event handlers
* Whenever testing default values of properties in components, mixins, routes,
etc, you **SHOULD** assert the properties in the same alphabetical order as
they appear in the unit under test.  This makes it easier to find the related
assertion, confirm one hasn't been missed, etc.

Of course there are always exceptions...

* if an action or method only accesses or affects the DOM then it does not need
to be Unit tested and Integration test only coverage is preferred
* an action or method, in addition to accessing or affecting the DOM, which
also interacts with properties on the component, may need to also be Unit tested
    * if the properties being set only pertain to the rendering of DOM, and not
    used by another other logic flow of the component, then Integration test
    only coverage is preferred
    * if the properties being set pertain to the rendering of the DOM and are
    also used by other logic flow the action or method should also be Unit
    tested
        * it is recommended that the logic of the action or method be broken
        into separate method (calls) to encompass more discreet units of work.
        Doing so will then make it very easy to either Unit or Integration test
        an action or method without having to exercise it via both, having to
        contrive data paths to get to the parts desired to be tested.

### Component Integration Tests

* **MUST** test:
    *  default rendered state of the component, including:
        * default classes applied
        * default properties/attributes applied
        * default characteristics of any mixins used by the component
    * any properties that can be provided as different values that cause a DOM
    change
    * any bound values
        * do not need to exercise the entire range of possible logic - this is
        done in the unit test.  Instead, this testing ensures that the template
        has bound things together correctly
    * actions
        * do not need to test the logic of the action - this is done in the
        unit test.  Instead, this testing ensures that the template has bound
        things together correctly
    * multiple rendered instances of the component in a template
        * ensures there is no pollution of references between the instances.
        The second rendered instance of the component should be consistently
        exercised while the first one is monitored to ensure there are not
        unintended side effects.  Such unintended side effects are such that a
        value set on a property on the second rendered instance should not be
        reflected in the first rendered instance.
* **MUST** reference a component under test via `this.$( '>:first-child' )`
rather than via a class name when appropriate, which is the majority of the
time.
    * Not using a classname removes the too-tight coupling between the styling
    used and the functionality provided by the component.
    * When this is not always appropriate is when you are rendering multiple
    components and need to access ones beyond `first-child`.
* **MUST** use `find()` for all selections, whether via an element type (i.e.
'span') or another selector (i.e. '.className')

Of course there are always exceptions...

* multiple rendered instances do not always have to be tested.  If the
component does not contain any logic that references the rendered instance via
`this.$()` then it is not necessary.


### Rendering Templates

* Template strings **MUST** be tagged/prefixed with `hbs` instead of compiled
with `Ember.HTMLBars.compile`
* If a template needs to be registered on the registry it **MUST** also be
unregistered at the conclusion of its use

[See Example](#example-rendering-templates)


### Example: Testing dependent keys

```javascript
// unit test

test( 'Dependent keys are correct', function( assert ) {
    const component = this.subject();

    const currentLabelDependentKeys = [
        'label',
        'pending',
        'pendingLabel'
    ];

    const sizeClassDependentKeys = [
        'size'
    ];

    assert.deepEqual(
        component.currentLabel._dependentKeys,
        currentLabelDependentKeys,
        'Dependent keys are correct for currentLabel()'
    );

    assert.deepEqual(
        component.sizeClass._dependentKeys,
        sizeClassDependentKeys,
        'Dependent keys are correct for sizeClass()'
    );
});
```

### Example: Testing mixins are mixed in

```javascript
// unit test

import InputBasedMixin from 'your-library/mixins/input-based';

test( 'Expected Mixins are present', function( assert ) {
    assert.ok(
        InputBasedMixin.detect( this.subject() ),
        'The input-based mixin is present'
    );
});
```

### Example: Rendering templates

```javascript
// unit or integration test

import hbs from 'htmlbars-inline-precompile';

test( 'Testing something needing template', function( assert ) {
    const template = hbs`
        {{#some-component label="One" name="one"}}
            One
        {{/some-component}}
    `;

    this.registry.register( 'template:test-template',  template );

    ...

    this.registry.unregister( 'template:test-template' );
});
```

### Example: Asynchronous testing

```javascript
// unit or integration test

test( 'Testing asynchronous behavior', function( assert ) {
    assert.expect( 2 );

    const done = assert.async();

    this.$( '.someSelector' ).queue( () => {
        assert.strictEqual(
            ...
        );

        assert.ok(
            ...
        );

        done();
    });
});
```
