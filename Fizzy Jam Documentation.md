# Fizzy Jam Documentation
> Read the latest doc at [GitHub](https://github.com/huangyuzhang/Fizzy-Jam/).

Fizzy Jam is an out-of-the-box JAMStack web app practice.

It's much more than a starter. 😉

## 🤔 Philosophy

1.  Everything lives in a git repo. This means you can host the site on GitHub, rather than paying monthly fees for web servers and database.
2.  An user-friendly yet pre-configured CMS. This allows you to focus on creating wonderful content, not the architecture or code.
3.  Decoupled everywhere. Customize the site is fun by adding micro-servicesll.

## 🍹 Features & Usage

### LOGO & ICON

LOGO elements will be shown by the following priority:

1.  **LOGO**: if the site LOGO is uploaded;
2.  **ICON + Site Name**: if the site ICON is uploaded;
3.  **Site Name**: if site name is defined.

> custom: `_includes/partial/header.njk`

### Routing

By default, Eleventy will generate HTML files based on the file structure in the `src` folder. That is, all entries inside `src/collections/post/` folder will be generated to `_site/post/`. So posts can be viewed at `www.yourdomain.com/post/post-slug/index.html`.

> custom: `collections/<collection_name>/files.md`

However, you may define `permalink` in the frontmatter of each post or in `<collection_name>.json` for all files within the same folder. For example, in this project, posts are stored in `src/collections/post`. So we can define all posts permalink as `"permalink": "post/{{slug}}/index.html"` by using `post.json` in the same folder.

> custom: `collections/<collection_name>/<collection_name>.json`

### Collection Entry Pages

-   **Single Author**: Customize the author page by populate name, avatar, background image, social account links, location and bio.
-   **Single Tag**: Contains the tag meta info and the posts with such tag.
-   **Single Post**: The post page.

### Single Pages

Page is one type of collections. All pages are stored in the `src/page/` folder. You need to edit the `permalink`s for each page to customize their URL.

There are several layouts you can choose from:

-   `single-page`: the post-like page, e.g. /about/
-   `list-post`: post listing page, e.g. /post/
-   `archive`: post archive page, e.g. /archives/
-   `list-tag`: tag listing page, e.g. /tag/
-   `list-author`: author listing page, e.g. /author/

> The `single-page` layout is sufficient for most of the time. However, you can always build your own layout for other purpose. PRs are welcome!

### Primary Tag

The first tag will be treated as the "primary tag" in display.

### Primary Author

The first author will be treated as the "primary author" in display.

### LaTeX Support

KaTeX is used to support block LaTeX. Write the LaTeX equation directly into the post content, surrounded by two dollar signs:

```
Write equation like this: $$ e = mc^2 $$.

```

Then it will be rendered as:

e\\=mc2 e = mc^2

### Table of Content

By turning on the "Table of Content" option on post editing page, a TOC (generate based on the H2 & H3 titles) will be shown on the right of the post content. Table of Content is not available on mobile devices.

### Code Highlight

Block code highlight is powered by Prism.js.

### Site Settings

Manually change site level settings.

#### Navigation

Add or remove items for the navbar or the footer area (coming soon!).

#### Settings

-   **Site Meta**: Site Name, LOGO, ICON, description, footer info ...
-   **Post Listing**: showFeatureImage, featureText
-   **SEO Settings**: SEO title, Keywords
-   **Appearance**: main color, link hover color ...
-   **Components**: author box, code line numbers ...

### Comment System

TODO

### Other Features

1.  Open external links in new pages.
2.  Image Lazy Loading

## Project Structure

## Netlify CMS Configuration

> file: `src/admin/config.yml`

### Collections

There are 5 collections by default: `post`, `tag`, `author`, `page` and `settings`.

`post`, `tag`, `author` and `page` are folder collections, so they need folders to store files with the same format. `settings` is a file collection, its embedded setting files are stored in `src/_data/`.

> more on [Collection Types - Netlify CMS](https://www.netlifycms.org/docs/collection-types/)

`tag` and `author` are relation collection used in post collection. So you need to first add new tags and authors before selecting them while editing a post.

#### Collection Slug

Slugs are used in routes, URLs and treated as the "id"s of entries. For example, the slug for a tag will be used in relation id stored in post files. It will be used also in generating the tag page: `/tag/{{slug}}`.

A slug only allows to contain "0~9", "a~z", "-", "\_"and"."(not start or end with"-","\_"and".").

## Eleventy Configuration

> file: `.eleventy.js`

Several JavaScript functions are introduced, so they can be used in templates.

| JavaScript Function         | Nunjucks Syntax | Explain                  |                                           |
| --------------------------- | --------------- | ------------------------ | ----------------------------------------- |
| `string.split("seperator")` | \`{{ myString   | split(",") }}\`          | Seperate a string with defined seperator. |
| `array.includes(item)`      | \`{{ myArray    | includes(item) }}\`      | Check whether an item is in an array.     |
| `str.substring(start,end)`  | \`{{ myString   | includes(start,end) }}\` | Slice a range of characters of a string.  |

## Nunjucks Highlights

-   Nunjucks passes parameters to included templates.

    > example in `single-tag.njk`, we included `loop-post.njk`. So the `{{slug}}`(tag slug) in single-tag will pass to loop-post, which allows us to filter the posts has the tag: `{{slug}}`.

## Stacks

-   Netlify CMS (git-based CMS)
-   Eleventy (static-site generator, Nunjucks as the template engine)
-   Bulma (CSS Framework)
-   Components
    -   Swiper Slider(TODO)
    -   KaTex (LaTeX support)
    -   Prism.js (Code Highlight)
    -   Table of Content

## Limitations & Known Issues

**All data lives in the `src` folder, the Netlify CMS has its limitations on respond to certain relation modifications.**

-   Duplicate entries may cause problems when generating static pages.
-   Delete tags(collection item) or change tag slugs will not remove the corresponding tag items in post files(.md).

    > Removed tags are not displaying in front-end, so users won't see those tags. However, still recommend not to change the tag slugs after creation.

## 📝 Changelog

See [CHANGELOG.md](https://blog.cvbox.org/post/doc/CHANGELOG.md)

## 🍻 Contributors

See [Contributors](https://github.com/huangyuzhang/Fizzy-Jam/graphs/contributors)

## 📍 Roadmap

To know the future planning of this project, please visit our [Roadmap](https://github.com/huangyuzhang/Fizzy-Jam/projects/3).

## Resources

-   [Fizzy Jam GitHub Repo](https://github.com/huangyuzhang/Fizzy-Jam/)
-   [Netlify CMS Docs](https://www.netlifycms.org/docs/)
-   [Eleventy Docs](https://www.11ty.dev/docs/)
-   [Nunjucks Templating Docs](https://mozilla.github.io/nunjucks/templating.html)
-   [Nunjucks VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=ronnidc.nunjucks)

## 💡 Contributing

1.  Fork it (maybe star this too?)
2.  Create your feature branch (`git checkout -b feature-fooBar`)
3.  Commit your changes (`git commit -m 'Add something'`)
4.  Push to the branch to origin (`git push origin feature-fooBar`)
5.  Create a new Pull Request to `dev` branch here
6.  Wait for code review and modify if necessary

## Roadmap & TODOs

The priority of the list below is based on the number of requests.

-   \[x] Minify Assets (HTML & CSS)
-   \[x] 404 Page
-   \[ ] Pagination
-   \[ ] Comment System
-   \[ ] Markdown highlight to mark `==highlight==` → `<mark>highlight</mark>`
-   \[ ] Search
    -   [Adding Search to your Eleventy Static Site with Lunr](https://www.raymondcamden.com/2019/10/20/adding-search-to-your-eleventy-static-site-with-lunr)
-   \[ ] i18n
-   \[ ] Night Switch
-   \[ ] Markdown Footnotes 
    [https://blog.cvbox.org/post/doc/](https://blog.cvbox.org/post/doc/) 
    [https://blog.cvbox.org/post/doc/](https://blog.cvbox.org/post/doc/)
