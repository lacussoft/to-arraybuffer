# LacusSoft :: to-arraybuffer

![NPM Latest Version](https://img.shields.io/npm/v/@lacussoft/to-arraybuffer)
![Downloads Count](https://img.shields.io/npm/dm/@lacussoft/to-arraybuffer.svg)
![Bundle Size](https://packagephobia.now.sh/badge?p=@lacussoft/to-arraybuffer)
![Last Update Date](https://img.shields.io/github/last-commit/juliolmuller/to-arraybuffer)
![Project License](https://img.shields.io/github/license/juliolmuller/to-arraybuffer)

Promise based function to generate **ArrayBuffer** objects for files - commonly required by web services like the SharePoint REST API.

## Browser Support

![Chrome](https://raw.github.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/src/safari/safari_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Edge](https://raw.github.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png) |
--- | --- | --- | --- | --- | --- |
Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | 11 ✔ |

## Installation

```bash
$ npm install @lacussoft/to-arraybuffer
```

## Import

```js
// ES Modules
import toArrayBuffer from '@lacussoft/to-arraybuffer'

// Common JS
const toArrayBuffer = require('@lacussoft/to-arraybuffer')
```

or import it through your HTML file, using CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/@lacussoft/to-arraybuffer@latest/dist/to-arraybuffer.min.js"></script>
```

## Usage

You can use various parameter types to reference your HTML input element holding the file, as well as **Blob** instances you may generate on the fly. As the process of generating **ArrayBuffer** is computationally expense, the result is not the ArrayBuffer itself, but a promise to it, so consider asynchronous approach to work with that.

```html
<input id="attachment" type="file"  />
<script type="text/javascript">

    const inputEl = document.getElementById("attachment")
    inputEl.addEventListener('change', async (ev) => {

        // Use a query selector directly
        const arrBuffer = await toArrayBuffer('#attachment')

        // Use the HTML element directly (must be of type "file")
        const arrBuffer = await toArrayBuffer(ev.target)

        // Use the element attribute that stores the FileList (only the first one will be converted)
        const arrBuffer = await toArrayBuffer(ev.target.files)

        // Use the element specific file within the FileList (great if you have a multi-file input)
        const arrBuffer = await toArrayBuffer(ev.target.files[0])

        /* do stuff */
    })

    // or if you got a Blob object
    const myBlob = new Blob(['Hello, world'], { type: 'text/plain' })
    toArrayBuffer(myBlob).then((arrBuffer) => /* do stuff */)

</script>
```

However, keep in mind that the function handles one single file, so by referencing an **HTMLInputElement** or its **FileList** attribute will only generate the **ArrayBuffer** for the `el.files[0]`. If you are working with multi-file input, you must iterate over the **FileList** object.

```html
<input id="attachments" type="file" multiple="true" />
<script type="text/javascript">

    const input = document.getElementById("attachments")
    const promises = []

    for (const file of input.files) {
      promises.push(toArrayBuffer(file))
    }

    Promise.all(promises).then((arrBuffers) => {
        /* do stuff */
    })

</script>
```
