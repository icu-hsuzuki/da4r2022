Welcome! 

This book is created using a minimal example of a book based on R Markdown and **bookdown** (https://github.com/rstudio/bookdown). 

1. Create a book using bs4_book style of a book  as a new project in RStudio.
2. Rename the  _book folder created as a default to docs folder
3. Edit _bookdown.yml to add
  - output_dir: docs
4. @GitHub: Create a repo in GitHub with the same name as the project.
5. Using Terminal: 
  5.1. git init
  5.2. git add README.md
  5.3. git commit -m "first commit"
  5.4. git branch -M main
  5.5. git remote rm origin
  5.6. git remote add origin git@github.com:icu-hsuzuki/da4r2022.git
  5.7. git push -u origin main
6. Restart R Studio
7. Build Book
8. Git: Commit "first build" > Push
9. @GitHub: Pages > main > docs

Another PC

1. Login to GitHub account
2. Copy SSH address under Code>Clone
3. Create a new project using Version Control:Git with the SSH address by setting the directory name
4. Edit README.md and test Git Commit and Push

Additional resources:

The **bookdown** book: https://bookdown.org/yihui/bookdown/

The **bookdown** package reference site: https://pkgs.rstudio.com/bookdown
