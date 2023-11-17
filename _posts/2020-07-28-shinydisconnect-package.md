---
title: "{shinydisconnect}: Show a nice message when a Shiny app disconnects or errors"
tags: [rstats, packages, shiny, shinydisconnect]
share-img: "/assets/img/blog/shinydisconnect/shinydisconnect.png"
thumbnail-img: /assets/img/blog/shinydisconnect/shinydisconnect-thumbnail.png
permalink: /blog/shinydisconnect-package/
date: 2020-07-28 11:00:00 -0400
gh-repo: daattali/shinydisconnect
gh-badge: [follow, star]
---

Have you ever noticed how an error in your Shiny app looks very different when it happens locally (in RStudio on your laptop) compared to when it happens in production (in shinyapps.io or Shiny Server or Connect)? Locally, when a Shiny app breaks, you just get a grey screen. But when a deployed app breaks, you also get a little strip that says "Disconnected from server. Reload."

[![shiny default message](/assets/img/blog/shinydisconnect/shinydisconnect-default-message.png){: .center }](/shinydisconnect/shinydisconnect-default-message.png)

You don't have any control over that message's text or position, and you don't have a way to get that message to appear both locally and in deployed apps.

Well, at least you didn't *until now*. The new [{shinydisconnect}](https://github.com/daattali/shinydisconnect) package solves exactly these two issues, by allowing you to show a customized (and pretty!) message when a Shiny app disconnects or errors, regardless of where the app is running.

You can visit the [GitHub page](https://github.com/daattali/shinydisconnect/) or check out a demo online:

<div style="text-align:center;">
<a class="btn btn-lg btn-cta" href="https://daattali.com/shiny/shinydisconnect-demo/">Check out a demo</a>
</div>

# Table of contents

- [Background](#background)
- [Overview](#overview)
- [Examples](#examples)
- [How to use](#usage)

## Background {#background}

When I created [CRANalerts](https://cranalerts.com/) a few years ago, I wanted to make it look beautiful and not at all like a Shiny app. After investing a lot of time on making it look great, there was still one last thing I wasn't happy about: that dreadful "Disconnected from server" message. It didn't show up when I was developing the app locally, and it wasn't really part of the shiny app - it's actually the shiny server that creates it. It was just *so* ugly and not user-friendly.

I searched the internet for a solution, and there was none. There were other people asking for solutions, but none offered. I couldn't just let it go, and after many anxious hours of thinking I may have to settle for something I don't like, I eventually found a way to customize it. Behold **the customized Shiny error message**!

[![shinydisconnect simple look](/assets/img/blog/shinydisconnect/shinydisconnect-simple.PNG){: .center }](/assets/img/blog/shinydisconnect/shinydisconnect-simple.PNG)

Ever since then I've used this trick in every shiny app I wrote. And a couple months ago I finally took the time to make a proper package out of it so that everybody could benefit from it.

## Overview {#overview}

A Shiny app can disconnect for a variety of reasons: an unrecoverable error occurred in the app, the server went down, the user lost internet connection, or any other reason that might cause the shiny app to lose connection to its server.

[{shinydisconnect}](https://github.com/daattali/shinydisconnect) allows you to add a nice message to the user when the app disconnects.  The message works both locally (running Shiny apps within RStudio) and on Shiny servers (such as shinyapps.io, RStudio Connect, Shiny Server Open Source, Shiny Server Pro). See the [demo Shiny app](https://daattali.com/shiny/shinydisconnect-demo/) online for examples.

## Examples {#examples}

For interactive examples and to see all the features, [check out the demo app](https://daattali.com/shiny/shinydisconnect-demo/).

**Example 1: basic usage ([code](https://github.com/daattali/shinydisconnect/blob/master/inst/examples/basic/app.R))**

[![basic screenshot](/assets/img/blog/shinydisconnect/basic.png)](/assets/img/blog/shinydisconnect/basic.png)

**Example 2: using parameters ([code](https://github.com/daattali/shinydisconnect/blob/master/inst/examples/advanced/app.R))**

[![advanced screenshot](/assets/img/blog/shinydisconnect/advanced.png)](/assets/img/blog/shinydisconnect/advanced.png)

**Example 3: full-width and vertically centered ([code](https://github.com/daattali/shinydisconnect/blob/master/inst/examples/special/app.R))**

You can also use `disconnectMessage2()` to get a similar message box to this one.

[![special screenshot](/assets/img/blog/shinydisconnect/special.png)](/assets/img/blog/shinydisconnect/special.png)

**Example 4: hide the app**

This can be achieved by setting the opacity to 1 (fully opaque).

[![special screenshot](/assets/img/blog/shinydisconnect/special2.PNG)](/assets/img/blog/shinydisconnect/special2.PNG)

## How to use {#usage}

The package only has two functions: `disconnectMessage()` and `disconnectMessage2()`.

Call `disconnectMessage()` anywhere in a Shiny app's UI to add a nice message when a shiny app disconnects. `disconnectMessage()` has many parameters to modify the text, position, and colours of the disconnect message.

Note that it's not possible to distinguish between errors and timeouts - they will both show the same message.

Without using this package, a shiny app that disconnects will either just show a greyed out screen if running locally (with no message), or will show a small message in the bottom-left corner that you cannot modify when running in a server.

Basic usage:

```
ui <- fluidPage(
  disconnectMessage(),
  actionButton("disconnect", "Disconnect the app")
)
server <- function(input, output, session) {
  observeEvent(input$disconnect, {
    session$close()
  })
}
shinyApp(ui, server)
```

You can use `disconnectMessage2()` as a pre-configured version of `disconnectMessage()` with a specific set of parameters.
