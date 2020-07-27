[Blazor]
# Using Blazor to GitHub Page explained
## Why is this so complicated?

[GitHub Pages](https://github.io/) is not very suitable for own website hosting. It's more suitable for documentation, so it needs some settings when you want to use it (especially modern framework, I mean, JS framework too). SPA routing does not work well without settings. But GitHub Pages is not for your complicated personal website. It's more suitable for documentation, for example. -Basically "Git" is version management.

But why we use this? At least, I don't have any web hosting, I don't have money and this is far more trusty. If you are sure that you want Blazor for GitHub, follow these instructions:

![Overview of the solution](contents/files/blazor-to-github-page/0.svg)

## 1. Integrity fail

> Failed to find a valid digest in the 'integrity' ... SHA256...

There are [several issues](https://github.com/dotnet/aspnetcore/issues/19907) and solutions about this. [Here](https://github.com/dotnet/aspnetcore/issues/19796) are some good solutions you can try.

There are some possibilities:

### CRLF problem

The problem is it: every dll files are binary, but *.js (including blazor.webassembly.js, dontnet.xxx.js) are not binary. (By the way, CRLF means \r\n linebreak.)

![Git adds automatically CRLF to non-binary file, so the result doesn't match](contents/files/blazor-to-github-page/1.svg)

And the result is non-matching hash, because .js file is changed.

So before you adding `_framework`, **create `.gitattributes`** first, and fill it with:

> \* binary

This means everything is considered as binary, so don't touch any CRLF things to anywhere.

If your `_framework` files are already added before using this trick, use:
> git add --renormalize

### Still got wrong, just they have wrong hash

The simple solution of this problem for this just removing all and regenerate. Or you can find right hash, match them and reupload instead.

## 2. \_framework is ignored

[GitHub page uses jekyll as default](https://help.github.com/en/github/working-with-github-pages/about-github-pages-and-jekyll), which means, it applies rules from jekyll. And one of the rules is **ignoring all directories and files that starts with underscores (\_)**. And it results this kind of message on your console:

> GET https://xxx.github.io/yyy/_framework/blazor.webassembly.js net::ERR_ABORTED 404

The solution is simple: **Add empty `.nojekyll` file** on root of the GitHub repository.

## 3. Cannot load anything but index.html

If you're deploying to your GitHub website root (username.github.io), this one is not necessary. But if you don't, remember Blazor default template's index.html contains this inside &lt;head&gt; tag:

> &lt;base href=&quot;\/&quot; &gt;

&lt;base&gt; tag indicates the **base path of every relative paths**. If you use Blazor on your GitHub subpages, change it.

> &lt;base href=&quot;\/YourSubProjectName\/&quot; &gt;

Note that you cannot see the file in local instead if you do this in your project file.

## 4. Cannot access with given link

The site is almost ready, except it isn't accesible outside of index page - even from the history. Well, it's not Blazor-specific, but the problem thats every SPA framework encounters.

[Someone made good solution](https://github.com/rafrex/spa-github-pages) to this. You can just copy from [here](https://github.com/rafrex/spa-github-pages/blob/gh-pages/index.html) and [here](https://github.com/rafrex/spa-github-pages/blob/gh-pages/404.html), from &lt;script&gt; to &lt;\/script&gt;. But what is this, and why this works? It's idea is so simple, that you can write yourself without copying them. Here are some hints:

![the query string p does the trick by passing route](contents/files/blazor-to-github-page/2.svg)

When user requests to the site, GitHub site looks into the page if the directory and the file exists. It does mean that this trick doesn't work if the valid file exists on the GitHub page same as on the route path - in the picture, if you create file */posts/3* on GitHub page repo, GitHub will show it first.

If the page does not exist, it redirects to the `404.html`. JavaScript content of `404.html` extracts route from current URL (`window.location.href`). Then `404.html` redirects to `index.html` with parameter *p* (or whatever). *p* contains the routes.

`index.html` holds the [History.replaceState()](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState). This replaces the current URL to *p* value that received from `404.html`, before the body is loaded. Now the body recognizes that the route is */posts/3*.

## What changes to deploy

changing `\_framework` should work if you changed your C# logic or Razor files. If CSS file is changed, changing only CSS files work. Just be careful when index.html is changed - be aware of the settings above (3, 4).