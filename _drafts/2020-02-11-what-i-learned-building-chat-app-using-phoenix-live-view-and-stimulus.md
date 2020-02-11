---
title: What I Learned Building a Chat App Using Phoenix Live View and Stimulus
layout: post
---

I've been building  [a real time chat application][project] using [Phoenix Live View][Live View], [Stimulus], and [Trix] and I wanted to do a writeup on what I have learned so far.

My goal for this project is to learn more about [Live View], and see how it interoperates with other javascript libraries.

I wanted to keep dependencies to a minimum in order to reduce complexity, increase response time, and minimize bandwidth. I wanted the page to display without waiting for the javascript to load, and I wanted to explore what would be required to get this working with javascript disabled.


### Stimulus works great with Live View

Installing [Stimulus] in Phoenix was identical to adding it to Rails Webpack project. I was up and running after following the [Using Webpack](https://stimulusjs.org/handbook/installing#using-webpack) and [Using Babel](https://stimulusjs.org/handbook/installing#using-babel) sections of their installation documents.

Since [Stimulus] monitors the DOM for changes and instantiates any new controllers that are added, it works flawlessly with [Live View].

My main use case for [Stimulus] is use a MutationObserver to detect new messages being addeed by [Live View]. I use the scroll position to determine whether I should scroll the new message into view or whether the user is further up the page and I should maintain the current scroll position and display a notification that there is a new message.

### Javascript libraries that create DOM elements need special consideration

Instantiating [Trix] in my project required adding a call to `require("trix/dist/trix")` to my `app.js`

Using [Trix] with [Live View] proved to be a bit more difficult because [Live View] will blow away any DOM changes made on the client side. This can be resolved by wrapping the dynamic content in an element with the attribute `phx-update="ignore"`

In order to populate a form value with the output from [Trix] I tied the editor to an input using the `trix-editor` `input` attribute.

[Trix] will populate the editor with the value of the input when the editor is first instantiated, but when [Live View] cleared the value the input after the form was submitted this wasn't detected by [Trix] so the trix-editor and the form input became of of sync.

To solve this issue I used the [JS Hooks] feature of [Live View].

``` javascript
import {Socket} from "phoenix"
import LiveSocket from "phoenix_live_view"

let setupTrix = function() {
  const trix = document.querySelector(`trix-editor[input=${this.el.id}]`)
  if (trix) {
    trix.value = this.el.value
    if (trix.value.length > 0) {
      trix.focus()
      trix.editor.setSelectedRange(trix.editor.getDocument().getLength() - 1)
    }
  }
}

let Hooks = {}
Hooks.Trix = {
  mounted() { setupTrix.call(this) },
  updated() { setupTrix.call(this) }
}

let csrfToken = document.querySelector("meta[name='csrf-token']").getAttribute("content");
let liveSocket = new LiveSocket("/live", Socket, {params: {_csrf_token: csrfToken}, hooks: Hooks});
liveSocket.connect()
```

I created a hook called `Trix` and added `phx-hook="Trix"` to the input element `trix-editor` is tied to. Now when the dom is mounted or updated, [Trix] is populate with the input value. If the input value contains contain the Trix editor will also be focused and the cursor position will be moved to the end of the content.


### Form inputs nested under `noscript` tags work inconsistently

will always submit their values in Chrome, but only javascript is disabled in Firefox.

### "`flex-direction: column-reverse`" has problems

CSS property `flex-direction: column-reverse` enables creating a scrollable container with the newest content and the scroll position at the bottom. This seemed like a great way to create a chat view without javascript but it had a few fatal flaws

In Firefox, [`flex-direction: column-reverse` breaks scrolling](https://bugzilla.mozilla.org/show_bug.cgi?id=1042151). This bug was first reported in 2014 and no progress has been made. I went as far as building Firefox locally and debugging the issue with their [Layout debugger](https://developer.mozilla.org/en-US/docs/Mozilla/Debugging/Layout_Debugger) and `lldb`, but my appetite for fixing this bug was small because it wasn't the only problem affecting this property.

The other issue I ran into involved inconsistent scroll position across browsers. In Chrome, `scrollTop` is equal to 0 at top of the container and increases positively as you scroll down. In Safari, `scrollTop` is equal to 0 at the *bottom* of the container and increases *negatively* as you scroll up. Considering this wasn't the only issue I wasn't interested in performing browser detection to get my Stimulus controller working.

What I ultimately decided to do was use `flex-direction: column`, and include an HTML anchor in the url whenever I redirected the user. The anchor `#message-anchor` appears in the DOM below the newest message which causes the messages container to scroll to the bottom with and without javascript enabled. 

### 100vh does not work well on mobile

[project]: https://github.com/abstractcoder/livechat
[Trix]: https://github.com/basecamp/trix
[Live View]: https://hexdocs.pm/phoenix_live_view/Phoenix.LiveView.html
[Stimulus]: https://stimulusjs.org/
[JS Hooks]: https://hexdocs.pm/phoenix_live_view/Phoenix.LiveView.html#module-js-interop-and-client-controlled-dom