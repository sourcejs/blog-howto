# Using SourceJS Context Settings

Version: 0.5.3

[SourceJS](http://sourcejs.com) is a scalable platform that adapts to your codebase and helps following best modular development practices. With [0.5.3 release](https://github.com/sourcejs/Source/releases/tag/0.5.3), engine started to support context options for your nested catalogs and bundles (read about bundles set-up [here](../catalog-setup)).

Using catalog and spec level setting, it's possible to override almost any options that are used on page request. With `sourcejs-options.js` file inside the spec catalog, all child (including nested) specs will read the updated configuration with your changes. This file has exactly the same structure as your initial `sourcejs/user/options.js` configuration.


Engine also supports local page settings through `info.json`:

```json
{
    "title": "Spec Title",
    "sourcejs": {
        "plugins": {
            "plugins": "options"
        }
    }
}
```

Where `sourcejs` field also replicates the same options structure as in `sourcejs/user/options.js` and `sourcejs-options.js` files.

Read more about levels of engine configuration in the [official docs](http://sourcejs.com/docs/configuration).

## Examples

To change any configuration per catalog, and all the specs inside it, just add new `sourcejs-options.js` file inside the directory:

```
/user
    /specs								- default catalog folder for Specs
        /bootstrap          			- nested bundle with Bootstrap components
            /bootstrap-component1
                index.src.html
                info.json
            /bootstrap-component2
                ...
            sourcejs-options.js			- default template for describing Nav page
            index.src.html
            info.json
```

With your custom configuration inside:

```
{
    assets: {
        modulesEnabled : {
            clarifyInSpec: false,
            sectionFolding: false
        }
    }
}
```

This will override the configuration which engine gets on page construction phase. Options that you provide, will extend the initial configuration object.

### Define custom specFile

It's possible to override all the configuration groups inside context options, expect `core`. With latest restructuring changes, we separated `rendering` specific configuration into stand-alone group.

Following the [reference of options defaults](https://github.com/sourcejs/Source/blob/master/options.js), and knowing that `sourcejs-options.js` has exactly the same structure, we can override page entry point for specific catalog:

```
{
    rendering: {
        specFiles: [
            'index.custom',
            'readme.md',
            'README.md'
        ],
    }
}
```

Now on page load, engine will first try to parse `index.custom` spec, instead of default `index.src.html`. [Here](https://github.com/sourcejs/example-specs-showcase/blob/master/react/sourcejs-options.js) you will find a real life usage example of this use case.

## Plugins

Context options are also available for plugins and middlewares that you can write for SourceJS engine ([docs about writing own plugins](http://sourcejs.com/docs/api/plugins/)).

Every spec page `req` object, that you then catch from plugin [is extended](https://github.com/sourcejs/Source/blob/master/core/middleware/read.js#L92) with special `req.contextOptions` field. Which means that recommended way of configuring plugins is to use same options structure with own namespace:

```
{
    core: {},
    rendering: {},
    plugins: {
        myPlugins: {
           options: "some"
        }
    }
}
```

## Questions

If you have more questions feel free to reach us [by email](mailto:r@rhr.me) or just use our Gitter chat:

[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/sourcejs/Source)