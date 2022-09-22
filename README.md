# Stats preview card component solution

<h3 align="center">
  <strong>
    <a href="https://arlagonix.github.io/projects/stats-preview-card-component-main/">Open Demo in Github Pages</a>
  </strong>
</h3>

<p align="center">
  <img src="./images/results/desktop.jpg" width="100%">
</p>

<p align="center">
  <img src="./images/results/mobile.jpg" width="50%">
</p>

## ℹ️ About

This is a solution to the [Stats preview card component challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/stats-preview-card-component-8JqbgoU62).

See [Task.md](./Task.md) for more details about the task

- **Build out a stats preview card**.
  - `/design` - Folder with designs. Contains both a mobile and a desktop version of the design.
  - `/images` - Folder with assets. The assets are already optimized.
  - `style-guide.md` - File with the style information: color palette, fonts, etc.
- **Get it looking as close to the design as possible**.
- **Use any tools to like**. You can use any tools you like to help you complete the challenge\*\*. So if you've got something you'd like to practice, feel free to give it a go.
- **Your users should be able to**:
  - View the optimal layout depending on their device's screen size

## ⚙️ Tools

- **HTML5**
  - Semantic HTML
- **SASS**
  - Flexbox
  - Responsive design
  - BEM naming convention
  - Mobile first approach
- **Github Pages** - for hosting
- **Live Server** - VS Code extension that launches local servers
- **Live Sass Complier** - VS Code extension that transpiles SCSS/SASS files in CSS

## 💡 Features

- **Animated Gradient text**. The text (and links) is colored with animated gradient. And it is achieved with only one class!
- **Animated Gradient images**. The image is "colored" by animated gradient
- **Hover animation on image, links**. Hover on the image = it scales. Hover on links = shrink. Click on links = shrink even more. Made it already several times in other projects.
- **SASS**. That is my first experience of using SASS (`.scss` to be specific). And it was wonderful! Nesting, mixins, partials! Oh my! Decided to use `.scss` instead of `.sass` because the first variant helps to easily reuse the `.css` fragments and files found in the Internet
- **SASS file composition**. Divided code into separate files. See below the details about the structure
- **Mixins**. For flexblox, for gradient background. See examples below
- **Text components**. Similarly to how it is done in Figma I made a text component in BEM convention. I made so in many previous projects as an experiment. ALthough now I see that this experiment was rather successful
- **Normalization**. Found a `normalize.css` file in Github, included it into the project

## 🔗 Useful resources

- [Normalize.css](github.com/necolas/normalize.css)
- [Live server : VS Code extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- [Live SASS Complier : VS Code extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.live-sass)

## 📍 Additional information

### Mobile first

Tried this time to style at first the layout for small screen sizes, then style the desktop layout (via media queries). It felt quite unusual, didn't see any difference with desktop first approach that I previously used. Although the project is rather small, so it might be strange if I could notice any difference.

### SASS file structure

It required some time to properly set it up. I didn't come to this idea on my own, used one article (lost it) as a foundation for my file structure, left only the necessary folders, so it's rather simple

I put all the stiles in a `styles` directory. Here is the structure of the folder

- `styles.scss` - contains all `use`. Sass team recommend to use `use` instead of `import`
- `styles.css` - generated CSS. Thanks to the VS Code plugin
- `styles.scss.map` - generated by the plugin, don't really know what it does
- `components` - a folder with all components in sense of BEM convention
  - `_component-name.scss` - a SASS partial - the code of the component itself
- `global` - a folder with global styles
  - `_colors.scss` - Contains definition of all colors in SASS variables
  - `_fonts.scss` - Imports all colors, contains definitions of font-families in SASS variables
  - `_globals.scss` - Contains global variables and styles
  - `_mixins.scss` - Mixins declaration
  - `_normalize.scss` - copied from github.com/necolas/normalize.css

### Animated Gradient Background

```html
<span class="text text--type_stats-value text--gradient">12M+</span>
</div>
```

The key fragment here is the class `text--gradient`. Just apply it to any text and boom! magic - the text is colored in gradient colors. The corresponding class is defined in `_text.scss`:

```scss
@use "../global/mixins";

.text {
  // Skipping the code
  &--gradient {
    @include mixins.moving-gradient-bg;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}
```

And the mixin is defined in the `_mixins.scss`:

```scss
@use "../global/colors";

@mixin moving-gradient-bg {
  background: linear-gradient(
    -90deg,
    colors.$gradient-1 0%,
    colors.$gradient-2 46%,
    colors.$gradient-3 100%
  );
  animation: gradient 10s ease infinite;
  background-size: 400% 400%;
  background-attachment: fixed;
}

@keyframes gradient {
  0% {
    background-position: 0% 0%;
  }
  50% {
    background-position: 100% 100%;
  }
  100% {
    background-position: 0% 0%;
  }
}
```

### Flexbox mixin

Quite an easy one, but it helped to keep the code DRY as soon as I use flexbox quite often

```scss
@mixin flex($align-items, $justify-content, $flex-direction) {
  display: flex;
  align-items: $align-items;
  justify-content: $justify-content;
  flex-direction: $flex-direction;
}
```

### Text components

The point is that every single piece of text on the page is an instance of a text component (in BEM convention). It helps to place all the text style definitions in one place. Thus it allows you to control your text styles globally from one place

I borrowed that idea from Figma that allows to define libraries for text styles. I thought it might be a great idea to try something like that in CSS

```scss
.text {
  font-family: fonts.$font-family-standard;
  color: colors.$secondary-white-1;
  font-size: $font-size-nm;

  &--type_header {
    font-family: fonts.$font-family-header;
    text-align: center;
    font-size: $font-size-lg;
    color: colors.$main-white;

    @media (min-width: globals.$break-point-lg) {
      text-align: left;
      font-size: $font-size-lg-lg;
    }
  }

  &--type_paragraph {
    color: colors.$secondary-white-1;
    text-align: center;
    line-height: 200%;

    @media (min-width: globals.$break-point-lg) {
      text-align: left;
    }
  }

  &--type_stats-value {
    font-size: $font-size-md;
    font-weight: 700;
    color: colors.$main-white;

    @media (min-width: globals.$break-point-lg) {
      text-align: left;
      font-size: $font-size-lg;
    }
  }
```

### Root `.scss` file

btw that's how it looks like:

```scss
@use "global/normalize";
@use "global/globals";

@use "components/page";
@use "components/text";
@use "components/main";
@use "components/picture-container";
@use "components/card";
@use "components/stats-item";
@use "components/link";
```

## 👤 Author

- Frontend Mentor - [@GrbnvAlex](https://www.frontendmentor.io/profile/GrbnvAlex)
- Telegram - [@Arlagonix](https://t.me/Arlagonix)
- Github - [@arlagonix](https://github.com/arlagonix)
