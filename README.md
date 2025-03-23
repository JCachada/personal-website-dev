# personal-website-dev

MIT License. Personal website built as a way to refocus on what's important and support the tiny, readable internet.

This website is based on a branch of https://bearblog.dev/ built for the Hugo static site generator (https://gohugo.io/). It uses https://github.com/janraasch/hugo-bearblog.

To run the website, clone the repository and run `hugo serve` in a terminal. You must have `hugo` installed - check the quickstart guides linked above.

My old website, built with Gatsby, lives at: https://github.com/JCachada/personal-website-dev-old

## TinaCMS

For locally writing posts, the website uses TinaCMS. Install the packages via `npm install` and run `npx tinacms dev -c "hugo server -D -p 1313"` to start the admin server.

You can then access the CMS at `http://localhost:1313/admin`.