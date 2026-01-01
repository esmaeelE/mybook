# Create book with mdbook rust and publish to github pages

This repository contains a simple example of how to create a book using `mdbook`, a popular tool for creating books with Markdown, and publish it to GitHub Pages.

## Prerequisites

- Rust and Cargo installed. You can install them from [rustup.rs](https://rustup.rs/).
- `mdbook` installed. You can install it using Cargo:

    ```bash
    cargo install mdbook
    ```

## Steps to Create and Publish the Book

1. **Create a New mdBook Project**:

    ```bash
    mdbook init my-book
    cd my-book
    ```

2. **Edit the Book Content**:
    Modify the `src/SUMMARY.md` file to define the structure of your book.
    For example:

    ```markdown
    # Summary           
    - [Chapter 1](./chapter_1.md)
    ```

    Create `src/chapter_1.md` and add some content to it.
3. **Build the Book**:

    ```bash
    mdbook build
    ```

    This will generate the static files for your book in the `book/` directory.
4. **Publish to GitHub Pages**:
    - Create a new repository on GitHub for your book.
    - Initialize a new Git repository in your book directory:

    ```bash
    cd book
    git init
    git remote add origin https://github.com/username/my-book.git
    ```
