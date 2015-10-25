Routes for Polymer 1.0
======================

A router as simple as it can be. Written for Polymer 1.0 in under 150 lines of code with support for:

* Nested routes
* Lazy loading of pages and site-sections
* Animated page transitions
* Reverse URL resolution

Usage
=====

Getting started
---------------

Setting up `<iron-routes>` starts as simple as this.

```html
<iron-routes for="app">
  <iron-route path="/"></iron-route>
  <iron-route path="/login"></iron-route>
  <iron-route path="/product/:productId"></iron-route>
</iron-routes>

<iron-pages id="app">
  <section><h1>Welcome!</h1></section>
  <section><h1>Login</h1> <a href="/product/1">or purchase</a></section>
  <section params="{{params}}"><h1>Product No. {{params.productId}}</h1></section>
</iron-pages>
```

How does it work? `<iron-routes>` registers a pushstate router.
Both `<iron-routes>` and `<iron-pages>` are _page selectables_ (implementing `IronSelectableBehavior`)
and synchronize their `selected` attribute. In this case, `selected` is the index of a child element.
See [iron-pages](https://elements.polymer-project.org/elements/iron-pages) to find out
what can be done with _selectables_.

Multiple selectables
--------------------

Additional selectable elements such as a navigation can be bound via the `selected` attribute.
The example binds a `<paper-tabs>` to actively highlight the relevant page section.
Binding permanent site components such as a navigation bar has the advantage that they
are not reinstantiated upon each page navigation.

```html
<iron-routes for="app" attr-for-selected="section" selected="{{section}}">
  <iron-route path="/" section="home"></iron-route>
  <iron-route path="/careers/" section="careers"></iron-route>
  <iron-route path="/careers/job/:jobId" section="careers"></iron-route>
</iron-routes>

<paper-tabs attr-for-selected="section" selected="[[section]]">
  <paper-tab section="home">Home</paper-tab>
  <paper-tab section="careers">Careers</paper-tab>
</paper-tabs>

<iron-pages id="app">
  <section><h1>Welcome</h1></section>
  <section><h1>Careers</h1></section>
  <section><h1>Careers: Job</h1></section>
</iron-pages>
```

Nested routes
-------------

Using nested routes, the example can be further simplified.

```html
<iron-routes for="app" selected="{{selected}}">
  <iron-route path="/"></iron-route>
  <iron-routes path="/careers/">
    <iron-route path="/"></iron-route>
    <iron-route path="/job/:jobId"></iron-route>
  </iron-routes>
</iron-routes>

<paper-tabs selected="[[selected]]">
  <paper-tab section="home">Home</paper-tab>
  <paper-tab section="careers">Careers</paper-tab>
</paper-tabs>

<iron-pages id="app">
  <section><h1>Welcome</h1></section>
  <iron-pages>
    <section><h1>Careers</h1></section>
    <section><h1>Careers: Job</h1></section>
  </iron-pages>
</iron-pages>
```

Lazy loading
------------

While a site keeps growing, lazy HTML imports and lazy element instantiation may speed up the
initial loading. _(This feature is not yet working stably.)_

```html
<iron-routes for="app" selected="{{selected}}">
  <iron-route path="/"></iron-route>
  <iron-routes path="/careers/" href="elements/careers-elements.html">
    <iron-route path="/"></iron-route>
    <iron-route path="/job/:jobId"></iron-route>
  </iron-routes>
</iron-routes>

<iron-pages id="app">
  <section><h1>Welcome</h1></section>
  <iron-pages>
    <careers-home/>
    <careers-job/>
  </iron-pages>
</iron-pages>
```

Animating pages
---------------

`<iron-routes>` integrates with any selectable element.
You can make use of [neon animation](https://elements.polymer-project.org/elements/neon-animation) features
to nicely animate page transitions. Replace `<iron-pages>` by `<neon-animated-pages>`.

```html
<iron-routes for="app" selected="{{selected}}">
  <iron-route path="/"></iron-route>
  <iron-routes path="/careers/" href="elements/careers-elements.html">
    <iron-route path="/"></iron-route>
    <iron-route path="/job/:jobId"></iron-route>
  </iron-routes>
</iron-routes>

<neon-animated-pages id="app" entry-animation="fade-in-animation" exit-animation="fade-out-animation">
  <section><h1>Welcome</h1></section>
  <iron-pages>
    <careers-home/>
    <careers-job/>
  </iron-pages>
</neon-animated-pages>
```


Install
=======

Install iron-routes via bower:

    bower install iron-routes
    
Import it after Polymer:

```html
<link href="bower_components/iron-routes/iron-routes.html" rel="import"/>
```
