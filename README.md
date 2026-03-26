# My Personal Site

A black and white Jekyll site powered by Hyde theme.

## Features

- 📝 Blog
- 📚 Training course materials
- 📄 Resume
- ✉️ Contact info

## Setup

### 1. Install dependencies

```bash
bundle install
```

### 2. Run locally

```bash
bundle exec jekyll serve
```

Visit `http://localhost:4000`

### 3. Deploy to GitHub Pages

```bash
# Create repo on GitHub named: username.github.io
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/username.github.io.git
git push -u origin main
```

## Customize

1. Edit `_config.yml` - Update title, author info, URL
2. Edit `index.md`, `about.md`, `resume.md` - Your content
3. Add posts to `_posts/` - Format: `YYYY-MM-DD-title.md`
4. Edit `assets/css/hyde-black-white.css` - Styling

## Directory Structure

```
.
├── _config.yml           # Site configuration
├── _layouts/             # HTML templates
├── _posts/               # Blog posts
├── assets/css/           # Custom CSS
├── index.md              # Homepage
├── about.md              # About page
├── resume.md             # Resume page
└── training.md           # Training materials
```

## License

MIT
