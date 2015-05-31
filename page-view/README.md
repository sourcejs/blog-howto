# Configuring Page Views

Version: 0.5.3

Each page in [SourceJS](http://sourcejs.com) engine is wrapped into parent [EJS template](http://ejs.co/), which allows to configure custom `<head>` section and page markup the way you want. Those may include your linked project styles or even JS frameworks, that you need to render components on the spec pages.

Here are the default page templates:

* https://github.com/sourcejs/Source/blob/master/core/views/spec.ejs
* https://github.com/sourcejs/Source/blob/master/core/views/navigation.ejs

All the tags in the template are quite straightforward, and here's the special place, where `index.src.html` (or any other spec extension) content in automatically placed:

```html
...
<div class="source_main source_col-main" role="main">
    <%- content %>
</div>
...
```

Note: remember that EJS rendering flow also allows you [to use it custom templating logic](http://sourcejs.com/docs/base/#server-side-templating-engines) (includes, data import) in spec pages like `index.src.html` ([example](https://github.com/sourcejs/Source/blob/master/docs/test-specs/styles/index.src.html).

## Re-defining Views

Paths to default templates are easy configurable, as you can also set your own template per any page through `info.json`.

To override view paths globally change configuration in `sourcejs/user/options.js`. Following the [reference options object](https://github.com/sourcejs/Source/blob/master/options.js), just add the same field with custom value:

```
{
    rendering: {
        views: {
            spec: [
                '$(user)/my/custom/views/spec.ejs'
            ]
        }
    }
}
```

This will point the engine to seek for spec EJS template in `sourcejs/user/my/custom/views` dir. Other available placeholder tags:

* `$(sourcejs)` - dir to SourceJS engine root
* `$(context)` - dir where configuration file is located

### From info.json

To override views per page using `info.json`, just add proper field to it:

```json
{
    "title": "Page Title",
    "template": "$(context)/template.ejs"
}
```

Read more about `info.json` contents in [docs](http://sourcejs.com/docs/info-json/).

## Questions

If you have more questions feel free to reach us [by email](mailto:r@rhr.me) or just use our Gitter chat:

[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/sourcejs/Source)