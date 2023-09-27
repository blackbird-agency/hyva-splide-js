<!-- markdownlint-configure-file {
  "MD013": {
    "code_blocks": false,
    "tables": false
  },
  "MD033": false,
  "MD041": false
} -->

<div align="center">
  
# hyva-splide-js

[![Latest Stable Version](https://img.shields.io/badge/version-2.0.1-blue)](https://packagist.org/packages/blackbird/hyva-splide-js)
[![SplideJS Version](https://img.shields.io/badge/splidejs-4.1.3-purple)](https://github.com/Splidejs/splide/releases/tag/v4.1.3)
[![License: MIT](https://img.shields.io/github/license/blackbird-agency/hyva-splide-js.svg)](./LICENSE)


An implementation of [SplideJS library](https://splidejs.com/) in [Hyvä Theme for Magento 2](https://www.hyva.io/hyva-themes-license.html)

You no longer need to worry about how to implement SplideJS in your Magento 2 Hyvä Theme projects.</br>
No manipulations required, instant use after installation.

Splide is lazily loaded and does not affect performances accoding to [Hyvä documentation](https://docs.hyva.io/hyva-themes/writing-code/patterns/loading-external-javascript.html).

[How It Works](#how-it-works) •
[Installation](#installation) •
[Usage](#usage) •
[More modules](#more-modules)

</div>

## How It Works

The module simply loads SplideJS on all pages with at least one element in the DOM bearing the CSS class `splide`</br>
(the class required by SplideJS).

When the library has been loaded on the page, a state stored in the [Alpine.store](https://alpinejs.dev/globals/alpine-store) is updated, indicating that SplideJS is ready for use.

The state can also be used to force the library to be loaded at any time, here is an example using [forceLoad()](#example--usage-of-forceload)

## Installation

> ### Requirements
> - [Hyvä Magento 2 Theme](https://www.hyva.io/hyva-themes-license.html)

```
composer require blackbird/hyva-splide-js
```
```
php bin/magento setup:upgrade
```
*In production mode, do not forget to recompile and redeploy the static resources.*

## Usage

Once the module has been installed, simply add the HTML code required to create a slider, as described in the [SplideJS documentation](https://splidejs.com/guides/getting-started/#html)
```html
<section class="splide" id="my-slider">
  <div class="splide__track">
    <ul class="splide__list">
      <li class="splide__slide">Slide 01</li>
      <li class="splide__slide">Slide 02</li>
      <li class="splide__slide">Slide 03</li>
    </ul>
  </div>
</section>
```

Next, create a function to listen the [Alpine.store](https://alpinejs.dev/globals/alpine-store) state `is_loading` indicating that SplideJS has been loaded, and apply Splide to the HTML elements, as described in the [SplideJS documentation](https://splidejs.com/guides/getting-started/#applying-splide).

```html
<?php
use \Blackbird\HyvaSplideJs\Api\HyvaSplideJSInterface;
?>
<script>
    function myXData () {
        return {
            initSlider() {
                if (Alpine.store('<?= HyvaSplideJSInterface::HYVA_SPLIDE_JS ?>').is_loaded) {
                    new Splide('#my-slider', {
                        ...options
                    }).mount();
                }
            }
        }
     }
</script>
```
*You can specify any of the SplideJS options as shown [here](https://splidejs.com/guides/options/)*

Finally, set up the [x-data](https://alpinejs.dev/directives/data) directive and do not forget to call the previous function in an [x-effect](https://alpinejs.dev/directives/effect), to prevent Splide being applied until the library is loaded, and to allow it to be automatically applied when the library is loaded.

```html
<section class="splide" id="my-slider" x-data="myXData()" x-effect="initSlider">
  ...
</section>
```

### Full example

```html
<?php
use \Blackbird\HyvaSplideJs\Api\HyvaSplideJSInterface;
?>
<section class="splide" id="my-slider" x-data="myXData()" x-effect="initSlider">
  <div class="splide__track">
    <ul class="splide__list">
      <li class="splide__slide">Slide 01</li>
      <li class="splide__slide">Slide 02</li>
      <li class="splide__slide">Slide 03</li>
    </ul>
  </div>
</section>
<script>
    function myXData () {
        return {
            initSlider() {
                if (Alpine.store('<?= HyvaSplideJSInterface::HYVA_SPLIDE_JS ?>').is_loaded) {
                    new Splide('#my-slider', {
                        ...options
                    }).mount();
                }
            }
        }
     }
</script>
```
*You can specify any of the SplideJS options as shown [here](https://splidejs.com/guides/options/)*

### Example : usage of `forceLoad()`

Imagine the following case: you do not have an element with the default `splide` class in your DOM, and you want to add a slider using SplideJS when a user's action is triggered.

In this case, Splide won't be loaded by default on the page, you will have to explicitly request that Splide be loaded.

```js
Alpine.store('<?= HyvaSplideJSInterface::HYVA_SPLIDE_JS ?>').forceLoad()
```
or
```js
$store.<?= HyvaSplideJSInterface::HYVA_SPLIDE_JS ?>.forceLoad()
```
*To find out exactly which one to use, please see the official Alpine documentation for [$store](https://alpinejs.dev/magics/store) or for [Alpine.store](https://alpinejs.dev/globals/alpine-store).*

This will force the library to load on the page, even if no element has the `splide` class. You can then follow the classic [Usage](#usage) procedure to apply Splide.

## More modules
  
[hyva-photo-swipe](https://github.com/blackbird-agency/hyva-photo-swipe) : An implementation of PhotoSwipe library in Hyvä Theme for Magento 2, full-screen gallery sliders, zoomable and highly customizable.
