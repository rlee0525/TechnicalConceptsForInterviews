# Web Crawler

![WebCrawler Diagram](http://res.cloudinary.com/rlee0525/image/upload/v1491531624/500px-WebCrawlerArchitecture.svg_p9viyi.png)

## What is it?
- A Search Engine Spider also known as a crawler, Robot, SearchBot or simply a Bot
- A program that most search engines use to find what’s new on the Internet. (e.g. GoogleBot)
- It “crawls” the web and collects documents to build a searchable index for the different search engines. The program starts at a website and follows every hyperlink on each page.
- Everything on the web will eventually be found and spidered, as the “spider” crawls from one website to another.
- Search engines may run thousands of instances of their web crawling programs simultaneously, on multiple servers.

## Three steps
- First, the search bot starts by crawling the pages of your site.
- Second, it continues indexing the words and content of the site.
- Lastly, it visit the links (web page addresses or URLs) that are found in your site. When the spider doesn’t find a page, it will eventually be deleted from the index.
  - Some of the spiders will check again for a second time to verify that the page really is offline.

## How does it work?
- When a web crawler visits one of your pages, it loads the site’s content into a database. Once a page has been fetched, the text of your page is loaded into the search engine’s index, which is a massive database of words, and where they occur on different web pages.

## robots.txt
- The first thing a spider is supposed to do when it visits your website is look for a file called “robots.txt”.
- This file contains instructions for the spider on which parts of the website to index, and which parts to ignore.
- The only way to control what a spider sees on your site is by using a robots.txt file.
- All spiders are supposed to follow some rules, and the major search engines do follow these rules for the most part.
- Including a robots.txt file can request bots to index only parts of a website, or nothing at all.

## Web Indexing
- Web crawlers can copy all the pages they visit for later processing by a search engine which indexes the downloaded pages so the users can search much more efficiently.
- Crawlers can validate hyperlinks and HTML code. They can also be used for web scraping, data-driven programming.
