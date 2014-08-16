Periodically, various APIs in Ember.js may be deprecated. During a minor
release, for instance when upgrading from version 1.9 to 1.10, you may see new
deprecations fire in your codebase. Until a major revision such as 2.0 lands,
code firing such deprecations is still supported by the Ember community. After
the next major revision lands, the supporting code may be removed. This style
of change management is commonly referred to as [Semantic Versioning](http://semver.org/).

What follows is a list of deprecations introduced to Ember.js during the 1.x
cycle.

#### Global lookup of views (since 1.8)

Previous to Ember 1.8, it views would commonly be fetched from the global
scope:

```hbs
{{view App.SomeView}}
{{each itemViewClass=App.SomeView}}
```

Since Ember 1.8, views are more appropriately resolved on the application
via strings:

```hbs
{{view "some"}}
{{each itemViewClass="view"}}
```

They may also be fetched via a binding:

```hbs
{{view view.someViewViaTheCurrentView}}
{{each itemViewClass=someViewViaAControllerProperty}}
```

In general, it is recommended that your Ember application avoid accessing
globals from a template. If the code triggering this deprecation is being
fired from a library, that library my need to update its suggested usage.

One solution for such a library is to provide mixins instead of classes:

```JavaScript
// usage is {{view "list"}}
var App.ListView = Ember.View.extend(ListView);
```

A more advanced solution is to use an initializer to register the plugin's
views on the the application:

```JavaScript
// usage is {{view "list"}}
Ember.Application.initializer({
  name: 'list-view',
  initialize: function(application, container) {
    application.register('view:list', ListView);
  }
});
```

More details on how to register an Ember.js framework component are available
in the [initializer API documentation](/api/classes/Ember.Application.html#toc_initializers)
and the [dependency injection guide](/guides/understanding-ember/dependency-injection-and-service-lookup).

