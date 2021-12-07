# Automagic LazyLoader
## Make the web faster using above-the-fold and folded sections

Automatic optimization of pages in Browser using noscript, template and lazy script, CSS, image and section loading.

### Motivation:

Roughly 70% of the Web is still Legacy pages and CMS, but a lot of the newer browser optimizations are targeting only modern react, VUE, Angular applications.

The modern traditional CMS marketing site is having some interactive parts, Ads, tracker scripts, 1000s of DOM nodes and often a lot of bloat. Sounds familiar? But it could be so much faster!

Images should be lazy everywhere - except above the fold. DOM parsing alone accounts for a lot of time spent just rendering the page and increasing the time till the first paint significantly. Scripts are blocking visually seeing the page due to avoiding the flash of unstyled content effect.

For all of these problems solutions exist (lazy-loading, async scripts, ajax pagers), but they are hard to implement, difficult to tune and often need server side support.

What if there was an easy way to make every page render faster, download less resources and which might become a future browser standard to implement this in all CMS seamlessly?

Meet automagic-lazyload.js - a method for giving the browser hints for lazy-loading images, sections, scripts by just adding the polyfill script (inline) and a few simple custom HTML tags and attributes wrapped in noscript for 100% backwards compatibility.

### Proposal

A page then can look like:

```
<head>

<noscript class=„lazyload“>
<script class=„above-the-fold“ src=„important.js“ />
<script class=„lazyload“ src=„defer.js“ />

</noscript>

</head>
<body>
<noscript class=„lazyload“>
<above-the-fold>
</above-the-fold>
<below-the-fold>
 <folded-section>
   <div class=„section“>Hello World</div>
 </folded-section>
</below-the-fold>
</noscript>
</body>
```

The script will take the contents of the noscript and will unwrap above-the-fold and display it immediately with all important scripts.

It will then take `<below-the-fold>` once this part has been painted and process it first by ensuring any scripts are loaded async, then that all images (with sizes) (and picture elements) have the lazyload attribute and then it checks if the user has scrolled, yet. If they did, it will push the below-the-fold as is to the body to avoid flow problems. If not further processing is done:

What is in folded-section is converted into a template element and just shown when the view port is close to the element. So the nodes are just parsed and any CSS / scripts needed for the section are loaded WHEN it’s actually needed.

This means that a lot of processing time is saved and the page speed score increases dramatically.

This does not yet solve sidebars, but for these elements on the page (even ads) a preprocess script run could nicely fade them in, once their DOM and scripts are ready. Very similar to how placeholders are used within eg the BigPipe technology.

For this the proposal is a `<lazy-section>` with a `<lazy-section-placeholder>` that is loaded immediately and then replaced with the real markup. So a Modern Web App experience on any HTML page - show the main content first, show everything else later - just when needed.

This all has been prototyped on a Drupal site (which already allows granular scripts and styles) but can be used for any system that outputs HTML:

### About the author

Fabian is a CMS performance and caching wizard. He has improved the performance and perceived performance of the Web by bringing BigTech technologies like BigPipe, Placeholders, Cache Contexts and Tags to Drupal 7/8/9/10. He is one of the current maintainers of Drupal 7, a CMS still used by over half a million web sites and is always striving to make the Web itself faster.
