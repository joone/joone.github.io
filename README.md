# Hugo Site Setup Instructions

This README provides detailed instructions on how to set up a Hugo site on Ubuntu and macOS.

## Installation

### Ubuntu

To install Hugo on Ubuntu, use the snap package manager:

```bash
sudo snap install hugo
```

### macOS

On macOS, you will first need to install Homebrew if you haven't already. Follow the installation instructions on [https://brew.sh/](https://brew.sh/).

After installing Homebrew, you can install Hugo:

```bash
brew install hugo
```

## Setting Up Your Site

Navigate to your desired directory (e.g., `git`):

```bash
cd git
```

Create a new Hugo site:

```bash
hugo new site my-hugo-blog
```

This command creates a new Hugo site in `/home/git/my-hugo-blog`.

### Next Steps

1. **Add Content:**

   You can add content by creating new files:

   ```bash
   hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>
   ```

2. **Start the Server:**

   To view your site locally, start Hugo's built-in live server:

   ```bash
   hugo server
   ```

## Cloning a Hugo Theme Repository

Choose a theme from [Hugo Themes](https://themes.gohugo.io/) and download it into the `themes` directory.


Initialize a Git repository if you haven't already:

```bash
git init
```

For example, to add the Archie theme:

```bash
git submodule add git@github.com:joone/archie.git themes/archie
```

## Configure Your Site

1. **Copy the Example Site:**

   Copy the contents from the theme's `exampleSite` to your site's root:

   ```bash
   cp -R themes/archie/exampleSite/* .
   ```

2. **Modify `config.toml`:**

   Edit your site's configuration file:

   ```bash
   vi config.toml
   ```

## Creating Content

To write a new post:

```bash
hugo new posts/hello.md
```

## Managing Static Files

To add images or other static files, place them in the `static/images` directory.

For more detailed instructions and documentation, visit the [Hugo Quickstart Guide](https://gohugo.io/getting-started/quick-start/) and the [Hugo Documentation](https://gohugo.io/documentation/).


Adjust the file paths and commands as necessary to match your specific setup or preferences.


## Add images in your post
You can add images directly in the content folder alongside your post in Hugo, which is particularly useful for organizing your content and images together, especially when using page bundles. Hugo supports page bundles, where a post and its related resources like images are grouped together in the same directory. Here's how you can do it:

1. **Create a Page Bundle**: Instead of creating a single markdown file for your post, create a directory for your post under `content/posts/`. For example, if your post is named `hello.md`, create a directory named `hello` and rename your post to `index.md` inside this directory.

   ```
   content
   └── posts
       └── hello
           ├── index.md
           └── image.jpg
   ```

2. **Add Your Image**: Place your image (`image.jpg` in this case) inside the `hello` directory alongside `index.md`.

3. **Reference the Image in Your Post**: In your `index.md`, reference the image using a relative path. Since the image is in the same directory as your markdown file, you don't need to specify a full path from the root. You can reference it like this:

   - Using Markdown:
     ```markdown
     ![Alt text](image.jpg "Optional title")
     ```
   - Using HTML:
     ```html
     <img src="image.jpg" alt="Alt text" title="Optional title" />
     ```

By using this approach, the image is tightly coupled with the post, making it easier to manage and move around if necessary. When you build your site, Hugo understands the structure and correctly processes the files, ensuring the image is displayed with your post.