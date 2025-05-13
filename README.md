# AbstractCoder Jekyll Site

This is the source code for [abstractcoder.com](https://abstractcoder.com), a blog and resource site built with [Jekyll](https://jekyllrb.com/).

## Prerequisites

- [Ruby](https://www.ruby-lang.org/en/documentation/installation/) (version 2.5 or higher recommended)
- [Bundler](https://bundler.io/) (optional, but recommended)
- [Jekyll](https://jekyllrb.com/docs/installation/)

## Getting Started

1. **Install Jekyll** (if you don't have it):

   ```sh
   gem install jekyll bundler
   ```

2. **Clone this repository:**

   ```sh
   git clone https://github.com/abstractcoder/abstractcoder.github.io.git
   cd abstractcoder.github.io
   ```

3. **Install dependencies:**

   This project does not include a `Gemfile`, so you only need Jekyll and Bundler installed globally.

4. **Serve the site locally:**

   ```sh
   jekyll serve
   ```

   Or, if you want to specify a custom config:

   ```sh
   jekyll serve --config _config.yml
   ```

5. **View the site:**

   Open your browser and go to [http://localhost:4000](http://localhost:4000)

## Configuration

- Main configuration: [`_config.yml`](./_config.yml)
- Posts: [`_posts/`](./_posts/)
- Drafts: [`_drafts/`](./_drafts/)
- Layouts: [`_layouts/`](./_layouts/)

## Notes

- No custom plugins or Gemfile are used in this project.
- For more information, see the [Jekyll documentation](https://jekyllrb.com/docs/). 