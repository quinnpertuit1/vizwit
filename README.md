# VizWit

[![Join the chat at https://gitter.im/timwis/vizwit](https://badges.gitter.im/timwis/vizwit.svg)](https://gitter.im/timwis/vizwit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Stories in Ready](https://badge.waffle.io/timwis/vizwit.svg?label=ready&title=Ready)](http://waffle.io/timwis/vizwit)

An interactive data visualization tool. VizWit uses a [JSON config file](https://gist.github.com/601224472a5d53cbb908) to generate 
interactive charts that cross-filter one another. It currently supports data hosted in a Socrata Open Data portal, which includes 
cities such as [Philadelphia](http://opendataphilly.org), [Chicago](https://data.cityofchicago.org/), [San Francisco](https://data.sfgov.org/)
and many others. However, interactions with Socrata have been [abstracted](src/scripts/collections/socrata.js) to allow
for other data providers to be written (in theory).

[![screencast](http://i.imgur.com/4gTXNFK.gif)](http://vizw.it/?gist=51db593dc0537d1a3f05)

# Features
* Clicking on a chart cross-filters the other charts
* Multiple datasets on one page
* Configurable via [GUI](http://builder.vizwit.io)
* Fork configurations with a gist
* Pie chart
* Callout metric
* Scrolling bar charts
* Responsive / mobile-friendly
* Embeddable
* Free-text search on table
* 100% client-side
* Open source / extendable

# Examples
* [Business Licenses in Chicago](http://vizw.it/?gist=a304ce8fac9c14dfbf16) ([config](https://gist.github.com/a304ce8fac9c14dfbf16))
* [Philadelphia Parking Violations](http://vizw.it/?gist=601224472a5d53cbb908) ([config](https://gist.github.com/601224472a5d53cbb908))
* [Philadelphia Property Assessments](http://vizw.it/?gist=cbd84c256f1efe250b8e) ([config](https://gist.github.com/cbd84c256f1efe250b8e))
* [City-owned Properties in Philadelphia](http://vizw.it/?gist=b03fc4fb30e5c8265f6b) ([config](https://gist.github.com/b03fc4fb30e5c8265f6b))
* [Chicago Crime Incidents](http://vizw.it/?gist=51db593dc0537d1a3f05) ([config](https://gist.github.com/51db593dc0537d1a3f05))
* [Callout Cards](http://vizw.it/?gist=eec2c0f438ebc8fd67c6) ([config](https://gist.github.com/eec2c0f438ebc8fd67c6))

# Configuration
VizWit cards are configured via JSON objects. The layout is defined using size and order properties on those objects, generated by the
[Config Builder](http://builder.vizwit.io) (WIP). The resulting JSON file is then hosted on [gist.github.com](https://gist.github.com), 
a free and easy service to share code snippets. The Gist ID is then used in the VizWit URL (ie. `vizw.it/?gist=a304ce8fac9c14dfbf16`). 
This allows viewers to fork the Gist and build their own VizWit page.

[Configuration Documentation](https://github.com/timwis/vizwit/wiki/Configuration)

# Build your own
Visit the [builder](http://builder.vizwit.io) and add a few cards, then configure them using the wrench icon. When you're finished, click
**Export** and copy the configuration code into a new "gist" at [gist.github.com](http://gist.github.com). Once you click **Create**,
you should see your new "gist" and the "gist id" of random characters in the URL (ie. `813483da72ac781f8b13`). Use that gist id to go to
`http://vizw.it?gist=YOUR_GIST_ID`

If you'd like to share what you've created, [post an issue](https://github.com/timwis/vizwit/issues/new) with a link to it.

# Technology
VizWit is a client-side JavaScript application. The application is structured using Backbone. Charts are built using AmCharts. Maps are
built using Leaflet. Tables are built using DataTables. Tests (of which there are not nearly enough!) are run using Mocha. The interactive
layout is generated by Gridstack. The application is compiled using Browserify. See the full list of dependencies in 
[package.json](https://github.com/timwis/vizwit/blob/master/package.json#L21-L66)

# How it works
[vizwit.js](src/scripts/vizwit.js) is the primary chart-generating module. It takes a `container` selector and a `config` object and
uses them to initialize a view (bar chart, map, table, etc.). The views are passed a `collection` and a `filteredCollection`, which are
identical and allow the view to query the data provider (Socrata). Views generate the chart/map/table/etc. and listen for user interactions
on them. On an interaction, the view triggers the global event object with the filter (ie. `state=PA`), and all views that use the same 
dataset receive the event and use their `filteredCollection` to query the data provider with the filter passed through the event. By
using a separate collection, VizWit can show the "filtered value" and the "original value" side-by-side.

The actual entry point is either [layout.js](src/scripts/layout.js) or [embed.js](src/scripts/embed.js). layout.js fetches a gist, reads
its configuration, and creates a layout on the page. Then for each chart in the configuration, it calls `init()` from vizwit.js as 
described above. embed.js simply finds the parent container of the `<script>` tag that was embedded, and calls `init()` from vizwit.js
inside of it. This way of embedding allows the VizWit library to only be loaded once on the page with as many charts embedded in any
element ([example](http://vizw.it/embed-demo.html)).

# Development
* While developing, use `npm start` to automatically recompile when changes are made and run a web server at `localhost:8080`
* Use `npm run lint` to verify code style
* Use `npm test` to run unit tests
* Use `npm run build` for a production build, and then use `npm run deploy` to push the `dist/` directory to the `gh-pages` branch

# License
[GPL-2](LICENSE.md) (create an issue if that doesn't work for someone)