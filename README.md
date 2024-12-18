The goal of this repository is to have a quick fix for accessibility issues. The solutions are not perfect, nor are they supposed to be perfect. They are also sample implementations that you should be able to drop into any project to ensure some foundational functionality.

## Page Titles

[Demo.](https://yatil.github.io/sledgehammer-a11y/page-titles.html)

The Page titles script ensures that your page title always updates based on an element on the page. This allows you to change content in an SPA (Single Page Application) and have your title in sync.

**Note:** This script uses a pretty broad MutationObserver to detect changes n the page. You might want to adjust the `targetNode` to improve performance. Alternatively add the function to your function when the page has loaded and delete the function content from `const = targetNode` on.

### Script

```javascript
function setPageTitle(targetSelector = "h1") {

  document.querySelector("head title").innerText = document.querySelector(targetSelector).innerText;

  const targetNode = document.querySelector("body");

  const config = {
    attributes: true,
    childList: true,
    subtree: true
  };

  const callback = (mutationList, observer) => {
    for (const mutation of mutationList) {
      document.querySelector("head title").innerText = document.querySelector(targetSelector).innerText;
    }
  };

  const observer = new MutationObserver(callback);

  observer.observe(targetNode, config);
}
setPageTitle();
```

#### Parameters

- `setPageTitle(targetSelector = "h1")`
  - `targetSelector` is the selector that picks the element whose content is used for the title. If the selector applies to multiple elements, the first element is used. Default: `h1`.

## Skip Links

[Demo.](https://yatil.github.io/sledgehammer-a11y/skip-links.html)

This script adds a function to add skip links to the page. The skip links use a class and you can define their target and the label/accessible name of the skiplink.

### Script

```javascript
function addSkipLink(targetSelector = "main", skipLinkName = "Skip to Content") {

  if (!document.querySelector("[data-sledgehammer-a11y='skipLinks']")) {
    document.querySelector("head").insertAdjacentHTML("beforeend", "<style data-sledgehammer-a11y='skipLinks'>.skiplink { display: block; color: #fff; background-color: #000; border: 0; clip: rect(0, 0, 0, 0); height: 1px; margin: -1px; overflow: hidden; padding: 0; position: absolute; white-space: nowrap; width: 1px; }.skiplink:active,.skiplink:focus { clip: auto; height: auto; margin: 0; overflow: visible; position: static; white-space: inherit; width: auto; }</style>");
  }

  let linkTarget = document.querySelector(targetSelector);
  let id = linkTarget.getAttribute("id");
  if (!id) {
    id = crypto.randomUUID();
    linkTarget.setAttribute("id", id);
  }
  linkTarget.setAttribute("tabindex", "-1");

  let skipLink = document.createElement("a");
  skipLink.setAttribute("href", "#" + id);
  skipLink.classList.add("skiplink");
  skipLink.innerText = skipLinkName;
  document.querySelector("body").insertBefore(skipLink, document.querySelector("body>:not(.skiplink)"));
}
addSkipLink();
```

#### Parameters

- `addSkipLink(targetSelector = "main", skipLinkName = "Skip to Content")`
  - `targetSelector` is the selector that picks the element which the skip links skips to. Default: `main`.
  - `skipLinkName` is the content of the skip link. Default: `Skip to Content`.

### Source

The CSS is based on [HTML5Boilerplates .visuallyhidden class](https://github.com/h5bp/html5-boilerplate/blob/e5a7112cff6e894d5e859cf08747e6e2c5e1689e/dist/css/style.css#L100-L137).

## Visible Focus

[Demo.](https://yatil.github.io/sledgehammer-a11y/visible-focus.html)

This code snippet ensures that the keyboard focus is always visible.

### Script

```javascript
function forceVisibleFocus() {
  if (!document.querySelector("[data-sledgehammer-a11y='visibleFocus']")) {
    document.querySelector("head").insertAdjacentHTML("beforeend", "<style data-sledgehammer-a11y='visibleFocus'>:focus-visible { outline: 3px solid black !important; box-shadow: 0 0 0 6px white !important;}</style>");
  }
}
forceVisibleFocus();
```

#### Parameters

- None

### Source

[Sara Soueidan’s Universal focus indicator.](https://www.sarasoueidan.com/blog/focus-indicators/#a-%E2%80%98universal%E2%80%99-focus-indicator)
