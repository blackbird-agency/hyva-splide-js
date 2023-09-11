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

[![Latest Stable Version](https://img.shields.io/badge/version-1.0.0-blue)](https://packagist.org/packages/blackbird/hyva-splide-js)
[![SplideJS Version](https://img.shields.io/badge/splidejs-4.1.3-purple)](https://github.com/Splidejs/splide/releases/tag/v4.1.3)
[![License: MIT](https://img.shields.io/github/license/blackbird-agency/hyva-splide-js.svg)](./LICENSE)


An implementation of [SplideJS library](https://splidejs.com/) in [Hyvä Theme for Magento 2](https://www.hyva.io/hyva-themes-license.html)

You no longer need to worry about how to implement SplideJS in your Magento 2 Hyvä Theme projects.</br>
No manipulations required, instant use after installation.

[How It Works](#how-it-works) •
[Installation](#installation) •
[Usage](#usage)

</div>

## How It Works

The module simply loads SplideJS on all pages with at least one element in the DOM bearing the CSS class `splide`</br>
(the class required by SplideJS).

When the library has been loaded on the page, a custom event is launched, indicating that SplideJS is ready for use.

## Installation

> ### Requirements
> - [Hyvä Magento 2 Theme](https://www.hyva.io/hyva-themes-license.html)

```
composer require blackbird/hyva-splide-js
```

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

Next, listen for the custom event indicating that SplideJS has been loaded, and apply Splide to the HTML elements, as described in the [SplideJS documentation](https://splidejs.com/guides/getting-started/#applying-splide).

### Example : using `x-data`

```html
<?php
use \Blackbird\HyvaSplideJS\Api\HyvaSplideJSInterface;
?>
<script>
    function myXData () {
        return {
            eventListeners: {
                ['@<?= HyvaSplideJSInterface::EVENT_SPLIDE_LOADED ?>.window']() {
                    new Splide('#my-slider', {
                        ...options
                    }).mount();
                }
            }
        }
     }
</script>
```

You can specify any of the SplideJS options as shown [here](https://splidejs.com/guides/options/)

### Full example

```html
<?php
use \Blackbird\HyvaSplideJS\Api\HyvaSplideJSInterface;
?>
<div x-data="myXData()" x-bind="eventListeners">
  <section class="splide" id="my-slider">
    <div class="splide__track">
      <ul class="splide__list">
        <li class="splide__slide">Slide 01</li>
        <li class="splide__slide">Slide 02</li>
        <li class="splide__slide">Slide 03</li>
      </ul>
    </div>
  </section>
</div>
<script>
    function myXData () {
        return {
            eventListeners: {
                ['@<?= HyvaSplideJSInterface::EVENT_SPLIDE_LOADED ?>.window']() {
                    new Splide('#my-slider', {
                        ...options
                    }).mount();
                }
            }
        }
     }
</script>
```
