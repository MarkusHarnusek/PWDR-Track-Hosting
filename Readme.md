# Presentations with Markdown and reveal.js

You write your slides in [Markdown](https://www.markdownguide.org/basic-syntax/), just like a `Readme.md`. [reveal.js](https://revealjs.com/) shows them in the browser as a presentation.

Why not PowerPoint?

- Your slides are text: you version them with git, see every change in the diff and work as a team without "final_v3_really_final.pptx".
- Code examples get syntax highlighting automatically.
- The result is a web page and runs in any browser, including on GitHub Pages.

The example presentation in this folder ([slides.md](./slides.md)) shows every building block from this guide.

## 1. Requirements

- [Node.js](https://nodejs.org/) version 22 (LTS) or newer. Check with `node -v`.
- An editor, e.g. VS Code.

## 2. Try the example

```bash
cd 01-ProjectStart/revealjs
npm install
npm start
```

Your browser opens `http://localhost:8000`. Change something in `slides.md` and save: the page reloads by itself. Stop with `Ctrl + C`.

## 3. Create your own project

A project consists of only a few files:

```text
my-presentation/
├── images/        images for the slides
├── index.html     loads reveal.js and reads slides.md (copy once)
├── slides.md      your slides (the only file you edit)
├── package.json
└── .gitignore
```

1. Create the folder and install the packages:

   ```bash
   mkdir my-presentation
   cd my-presentation
   npm init -y
   npm install reveal.js
   npm install -D browser-sync
   mkdir images
   ```

   `reveal.js` is the presentation software. `browser-sync` is a small web server that reloads the browser whenever you save.

2. Copy [index.html](./index.html) from this folder into your project.

3. In `package.json`, replace the `"scripts"` block with:

   ```json
   "scripts": {
     "start": "browser-sync start --server --files \"index.html\" \"slides.md\" \"images/**\" --port 8000 --no-notify"
   }
   ```

4. Create a `.gitignore` file so the installed packages don't end up in the repository:

   ```text
   node_modules/
   ```

Anyone who clones your repository runs `npm install` and `npm start`.

## 4. Your first slide

Create `slides.md`:

```markdown
# Our Project

Anna Huber, Ben Koller

---

## Current situation

- The school tracks loan devices in Excel
- Nobody knows who has which laptop

---

## Goal

A web app that records every loan.
```

A line with `---` starts a new slide. It needs an empty line before and after it.

Start with `npm start` and move through the slides with the arrow keys.

## 5. Building blocks for slides

### Vertical slides

A line with `--` creates a sub-slide. You reach it with the down arrow. Useful for details you can skip when time is short.

```markdown
## Architecture

--

## Frontend

Angular

--

## Backend

Quarkus
```

### Reveal bullet points one by one

Add a comment after each bullet point that turns it into a "fragment":

```markdown
- first this <!-- .element: class="fragment" -->
- then this <!-- .element: class="fragment" -->
```

### Code with highlighted lines

After the language, put the lines to highlight in square brackets: `[3]` for one line, `[2-4]` for a range.

````markdown
```java [3]
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
````

Separated by `|`, the lines are highlighted one after another, the next one each time you move forward: `[1|2|3]`.

### Images

Images go into the `images` folder.

```markdown
![Architecture](images/architecture.png)
```

If the image is too large, set its height (`60vh` = 60 % of the screen height):

```markdown
![Architecture](images/architecture.png) <!-- .element: style="height: 60vh" -->
```

### Two columns

Markdown has no columns. For that, `index.html` defines the class `columns`. The empty lines around the Markdown content matter, otherwise it isn't converted.

```markdown
## Before and after

<div class="columns">
<div>

![Excel](images/excel.png)

</div>
<div>

- Excel list
- no history

</div>
</div>
```

### Tables

```markdown
| Role | Person |
| --- | --- |
| Product Owner | Anna |
| Scrum Master | Ben |
```

### Speaker notes

Only the presenter sees the notes. Everything after a line `Note:` up to the end of the slide is a note. Press `S` to open the speaker view in its own window: current slide, next slide, notes and a clock.

```markdown
## Goal

A web app that records every loan.

Note:
Tell the story about the lost laptop here.
```

### Background

A comment at the start of the slide:

```markdown
<!-- .slide: data-background-color="#1e3a5f" -->

## Thank you!
```

An image as the background of the whole slide:

```markdown
<!-- .slide: data-background-image="images/title.jpg" -->

## Welcome
```

### Theme

You set the look in `index.html`, in the line with `theme/white.css`. Instead of `white` you can use `black`, `league`, `beige`, `night`, `serif`, `simple`, `solarized`, `moon`, `dracula`, `sky` or `blood`. On a projector in a bright room, `white` and `simple` are the easiest to read.

## 6. Presenting

| Key | Effect |
| --- | --- |
| Arrow keys, space | move through slides |
| `F` | full screen |
| `S` | speaker view with notes |
| `O` or `Esc` | overview of all slides |
| `B` or `.` | black screen |
| `Alt` + click | zoom into an area |

## 7. Save as PDF

1. Open the presentation with `npm start`.
2. Add `?print-pdf` to the address: `http://localhost:8000/?print-pdf`
3. Print in **Chrome or Edge** (`Ctrl + P`): destination "Save as PDF", layout landscape, margins "None", background graphics on.

## 8. Publish online (GitHub Pages)

`index.html` loads reveal.js from `node_modules`, which is not part of the repository. For the online version you therefore load reveal.js from the internet (CDN): in `index.html`, replace every

```text
node_modules/reveal.js/
```

with

```text
https://cdn.jsdelivr.net/npm/reveal.js@6.0.2/
```

Then publish `index.html`, `slides.md` and the `images` folder. In the repository, choose the branch under *Settings → Pages*.

## 9. Troubleshooting

| Problem | Cause and fix |
| --- | --- |
| The page stays white | You opened `index.html` by double-clicking. The browser then isn't allowed to load `slides.md`. Always open it with `npm start`. |
| Everything is one single slide | The empty line before or after `---` is missing. |
| Text shows up as `<!-- .element ... -->` | The comment has to be on the same line as the bullet point. |
| The columns show `![Image](...)` instead of the image | The empty lines before and after the Markdown content inside the `<div>` are missing. |
| Image is missing | The file isn't in `images` or the name is misspelled (upper/lower case matters online). |
| Text runs off the bottom of the slide | Too much content: split it over two slides or add a vertical slide. |
| Port 8000 is in use | Enter a different number after `--port` in `package.json`. |

## 10. Further reading

- Markdown in reveal.js: https://revealjs.com/markdown/
- Everything about reveal.js (transitions, animations, layouts): https://revealjs.com/
- Markdown basics: https://www.markdownguide.org/basic-syntax/

## 11. Tip: Quarto

If you want more, have a look at [Quarto](https://quarto.org/docs/presentations/revealjs/). You also write Markdown there, and Quarto builds a reveal.js presentation from it. Columns, callout boxes, diagrams and code from real files are built in, without HTML. In return you have to install Quarto, and there is an extra build step. If you like, the same file can also produce PowerPoint or PDF slides.
