### [Example program: list links](https://jsoup.org/cookbook/extracting-data/example-list-links)
> Example program: list links
> 1. This example program demonstrates how to fetch a page from a URL; 
> 2. extract links, images, and other pointers; and 
> 3. examine their URLs and text.

Specify the URL to fetch as the program's sole argument.

```
package org.jsoup.examples;

import org.jsoup.Jsoup;
import org.jsoup.helper.Validate;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.IOException;

/**
 * Example program to list links from a URL.
 */
public class ListLinks {
    public static void main(String[] args) throws IOException {
        Validate.isTrue(args.length == 1, "usage: supply url to fetch");
        String url = args[0];
        print("Fetching %s...", url);

        Document doc = Jsoup.connect(url).get();
        Elements links = doc.select("a[href]");
        Elements media = doc.select("[src]");
        Elements imports = doc.select("link[href]");

        print("\nMedia: (%d)", media.size());
        for (Element src : media) {
            if (src.normalName().equals("img"))
                print(" * %s: <%s> %sx%s (%s)",
                        src.tagName(), src.attr("abs:src"), src.attr("width"), src.attr("height"),
                        trim(src.attr("alt"), 20));
            else
                print(" * %s: <%s>", src.tagName(), src.attr("abs:src"));
        }

        print("\nImports: (%d)", imports.size());
        for (Element link : imports) {
            print(" * %s <%s> (%s)", link.tagName(),link.attr("abs:href"), link.attr("rel"));
        }

        print("\nLinks: (%d)", links.size());
        for (Element link : links) {
            print(" * a: <%s>  (%s)", link.attr("abs:href"), trim(link.text(), 35));
        }
    }

    private static void print(String msg, Object... args) {
        System.out.println(String.format(msg, args));
    }

    private static String trim(String s, int width) {
        if (s.length() > width)
            return s.substring(0, width-1) + ".";
        else
            return s;
    }
}

org/jsoup/examples/ListLinks.java
```
### Example output (trimmed) 
```
Fetching http://news.ycombinator.com/...

Media: (38)
 * img: <http://ycombinator.com/images/y18.gif> 18x18 ()
 * img: <http://ycombinator.com/images/s.gif> 10x1 ()
 * img: <http://ycombinator.com/images/grayarrow.gif> x ()
 * img: <http://ycombinator.com/images/s.gif> 0x10 ()
 * script: <http://www.co2stats.com/propres.php?s=1138>
 * img: <http://ycombinator.com/images/s.gif> 15x1 ()
 * img: <http://ycombinator.com/images/hnsearch.png> x ()
 * img: <http://ycombinator.com/images/s.gif> 25x1 ()
 * img: <http://mixpanel.com/site_media/images/mixpanel_partner_logo_borderless.gif> x (Analytics by Mixpan.)
 
Imports: (2)
 * link <http://ycombinator.com/news.css> (stylesheet)
 * link <http://ycombinator.com/favicon.ico> (shortcut icon)
 
Links: (141)
 * a: <http://ycombinator.com>  ()
 * a: <http://news.ycombinator.com/news>  (Hacker News)
 * a: <http://news.ycombinator.com/newest>  (new)
 * a: <http://news.ycombinator.com/newcomments>  (comments)
 * a: <http://news.ycombinator.com/leaders>  (leaders)
 * a: <http://news.ycombinator.com/jobs>  (jobs)
 * a: <http://news.ycombinator.com/submit>  (submit)
 * a: <http://news.ycombinator.com/x?fnid=JKhQjfU7gW>  (login)
 * a: <http://news.ycombinator.com/vote?for=1094578&dir=up&whence=%6e%65%77%73>  ()
 * a: <http://www.readwriteweb.com/archives/facebook_gets_faster_debuts_homegrown_php_compiler.php?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+readwriteweb+%28ReadWriteWeb%29&utm_content=Twitter>  (Facebook speeds up PHP)
 * a: <http://news.ycombinator.com/user?id=mcxx>  (mcxx)
 * a: <http://news.ycombinator.com/item?id=1094578>  (9 comments)
 * a: <http://news.ycombinator.com/vote?for=1094649&dir=up&whence=%6e%65%77%73>  ()
 * a: <http://groups.google.com/group/django-developers/msg/a65fbbc8effcd914>  ("Tough. Django produces XHTML.")
 * a: <http://news.ycombinator.com/user?id=andybak>  (andybak)
 * a: <http://news.ycombinator.com/item?id=1094649>  (3 comments)
 * a: <http://news.ycombinator.com/vote?for=1093927&dir=up&whence=%6e%65%77%73>  ()
 * a: <http://news.ycombinator.com/x?fnid=p2sdPLE7Ce>  (More)
 * a: <http://news.ycombinator.com/lists>  (Lists)
 * a: <http://news.ycombinator.com/rss>  (RSS)
 * a: <http://ycombinator.com/bookmarklet.html>  (Bookmarklet)
 * a: <http://ycombinator.com/newsguidelines.html>  (Guidelines)
 * a: <http://ycombinator.com/newsfaq.html>  (FAQ)
 * a: <http://ycombinator.com/newsnews.html>  (News News)
 * a: <http://news.ycombinator.com/item?id=363>  (Feature Requests)
 * a: <http://ycombinator.com>  (Y Combinator)
 * a: <http://ycombinator.com/w2010.html>  (Apply)
 * a: <http://ycombinator.com/lib.html>  (Library)
 * a: <http://www.webmynd.com/html/hackernews.html>  ()
 * a: <http://mixpanel.com/?from=yc>  ()
```