# My blog

Welcome to the code of [Baran's blog](https://regal-shortbread-d385bf.netlify.app/)!

## Installation

### Pre-requisites

- [rbenv](https://github.com/carsomyr/rbenv-bundler) *(I recommend only installing this and skipping the rest of the pre-requisites)*
- [Ruby](https://www.ruby-lang.org/en/)
- [Bundler](https://bundler.io/)

### Steps to run the blog on your local machine

1. Clone the repo.
```bash
git clone https://github.com/cjxe/blog.git
```

2. Navigate to the folder.
```bash
cd blog
```

3. Install the `bundler` gem. If you have it globally installed, skip this step.
```bash
gem install bundler
```

4. Install the project's dependencies.
```bash
bundle install
```

5. Run the blog.
```bash
bundle exec jekyll serve
```

6. Navigate to the blog using your favourite browser.
```
http://localhost:4000
```

---

Made with [Klisé](https://github.com/piharpi/jekyll-klise) on Jekyll
