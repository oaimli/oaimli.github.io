- This is adapted from the project [TeXt theme](https://github.com/kitian616/jekyll-TeXt-theme)
- change records
- changed _includes/footer.html
- changed _includes/svg/logo.svg
- changed favicon.ico
- added files folder for PDF storage
- change about.md to index.md to make the root page be personal info.
- add folder blogs and move index.html into it, change navigation to /blogs/ to make Posts be the page of blogs.
- change _data/navigation.yml 
- delete header from _includes/header.html
- change _config, home->/blogs/  and  paginate_path: /page:num->paginate_path: /blogs/page:num
- change blogs/paper-list.md, this file can be included as the full version of paper list, https://oaimli.github.io/blogs/paper-list
- blogs should be put into _posts