# My Blog

A Jekyll-powered blog hosted on GitHub Pages, featuring the Just the Docs theme for clean navigation and excellent readability.

## Features

- Clean, responsive design with Just the Docs theme
- Built-in search functionality
- RSS feed support via jekyll-feed plugin
- SEO optimized with jekyll-seo-tag
- Markdown-based content
- Automatic deployment via GitHub Pages

## Local Development

### Prerequisites

- Ruby 2.7 or higher
- Bundler gem

### Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/Blog.git
   cd Blog
   ```

2. Install dependencies:
   ```bash
   bundle install
   ```

3. Run the local development server:
   ```bash
   bundle exec jekyll serve
   ```

4. Open your browser and visit:
   ```
   http://localhost:4000
   ```

The site will automatically reload when you make changes to files.

## Creating New Posts

1. Create a new file in the `_posts/` directory with the format:
   ```
   YYYY-MM-DD-title-of-post.md
   ```

2. Add the front matter at the top of the file:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: 2025-10-28 12:00:00 +0000
   categories: category-name
   author: Your Name
   ---
   ```

3. Write your content in Markdown below the front matter.

4. Save the file and the post will appear on your site.

## Project Structure

```
Blog/
├── _posts/              # Blog posts (YYYY-MM-DD-title.md format)
├── _site/               # Generated site (not in version control)
├── _config.yml          # Jekyll configuration
├── Gemfile              # Ruby dependencies
├── index.md             # Homepage
├── about.md             # About page
├── 404.html             # Custom 404 error page
├── CLAUDE.md            # Development workflow documentation
├── todo.md              # Project todo template
└── README.md            # This file
```

## Configuration

Edit `_config.yml` to customize:
- Site title, description, and author
- GitHub repository URL
- Theme settings
- Plugin configuration
- Navigation structure

Important: Update these values in `_config.yml`:
- `url`: Your GitHub Pages URL
- `baseurl`: Repository name (or empty for username.github.io)
- GitHub link in `aux_links`
- Author name and email

## Deployment to GitHub Pages

1. Create a new repository on GitHub named `Blog`

2. Add the remote and push:
   ```bash
   git remote add origin https://github.com/your-username/Blog.git
   git push -u origin main
   ```

3. Enable GitHub Pages:
   - Go to repository Settings > Pages
   - Source: Deploy from a branch
   - Branch: main / (root)
   - Save

4. Your site will be available at:
   ```
   https://your-username.github.io/Blog/
   ```

GitHub Pages automatically builds and deploys your site when you push changes to the main branch.

## Customization

### Theme Customization

The Just the Docs theme can be customized by:
- Editing `_config.yml` for theme settings
- Adding custom CSS in `assets/css/`
- Overriding theme layouts in `_layouts/`
- Customizing components in `_includes/`

See the [Just the Docs documentation](https://just-the-docs.github.io/just-the-docs/) for more options.

### Adding Pages

Create new `.md` files in the root directory with front matter:

```yaml
---
layout: default
title: Page Title
nav_order: 3
---
```

## Built With

- [Jekyll](https://jekyllrb.com/) - Static site generator
- [Just the Docs](https://just-the-docs.github.io/just-the-docs/) - Jekyll theme
- [GitHub Pages](https://pages.github.com/) - Hosting platform

## License

This project is open source and available under the [MIT License](LICENSE).

## Support

For issues or questions:
- Check the [Jekyll documentation](https://jekyllrb.com/docs/)
- Visit [Just the Docs documentation](https://just-the-docs.github.io/just-the-docs/)
- Review [GitHub Pages documentation](https://docs.github.com/en/pages)
