---
title: "Using the Screaming Frog SEO Spider to Generate Markdown at Scale"
url: "https://www.screamingfrog.co.uk/blog/generate-markdown-at-scale/"
date: "Tue, 21 Apr 2026 12:50:35 +0000"
author: "Mark Porter"
feed_url: "https://www.screamingfrog.co.uk/feed/"
---
<p>LLMs, RAG pipelines and various AI-powered tools are now a regular part of a lot of SEOs&#8217; workflows, and a common starting point for many of them is getting content out of web pages and into a clean, usable format at scale.</p>
<p>Markdown has emerged as the format of choice for most of these use cases. It&#8217;s lightweight, preserves the structure of the page, and is well understood by virtually every LLM out there. The challenge is that web pages are full of stuff you don&#8217;t want to carry across with the content. Navigation, cookie banners, ads, sidebars and footers are just a few examples, and stripping them out manually on every page is obviously not going to fly if you&#8217;re working with thousands of URLs.</p>
<p>Thankfully, the <a href="https://www.screamingfrog.co.uk/seo-spider/">Screaming Frog SEO Spider</a> can handle this for you using its Custom JavaScript feature. In this guide, we&#8217;ll walk through two methods for generating markdown of every page on a site during a crawl, and we&#8217;ll share a Python script that takes the SEO Spider&#8217;s CSV export and turns it into individual .md files for use downstream.</p>
<hr class="light" />
<h2 id="why-markdown">Why Markdown?</h2>
<img alt="" class="aligncenter size-full wp-image-360950" height="662" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/markdown.png" width="1519" />
<p>Before getting into the how, it&#8217;s worth briefly covering why this format has become such a common choice within AI workflows.</p>
<ul>
<li><strong>It&#8217;s much smaller than HTML.</strong> HTML is bloated with wrapper divs, inline styles, tracking pixels and all sorts of other code that doesn&#8217;t contribute anything useful to the content. Feeding this into an LLM burns through tokens unnecessarily, which directly translates to higher API costs and less room in the context window for the information you actually care about.</li>
<li><strong>It preserves the structure of the page.</strong> Headings, lists, links, bold and italic text are all retained, which makes it significantly easier for an LLM to understand what it&#8217;s looking at. It&#8217;s also a format that most modern LLMs have been heavily trained on.</li>
<li><strong>It&#8217;s become the de facto format for LLM ingestion.</strong> Most embedding pipelines, RAG frameworks and fine-tuning tools either expect markdown or convert to something close to it before chunking. Starting in markdown saves a step.</li>
<li><strong>It&#8217;s easy to read and review.</strong> A .md file can be opened in pretty much any text editor. Handy if you&#8217;re sense-checking a few thousand exported pages before feeding them into a pipeline.</li>
</ul>
<p>There are a fair few use cases for content in this format. You might be building a knowledge base for a chatbot on a client&#8217;s site, preparing training data for fine-tuning, running competitor analysis through an LLM, getting all examples of a writing style for creating new content, or keeping a clean plain-text archive of a site ahead of a migration. In all of these, getting to clean markdown is the first step.</p>
<hr class="light" />
<h2 id="markdown-for-bots">A Note on Serving Markdown to Bots</h2>
<p>Before getting into the how-to, it&#8217;s worth flagging an ongoing debate around all of this. Some site owners have started experimenting with serving raw markdown files to LLM crawlers, either alongside their normal HTML or as a replacement, on the basis that it&#8217;s cheaper to serve and easier for a bot to parse. Cloudflare even have a one-button solution that automatically converts your HTML to Markdown for requests from agents.</p>
<p>This approach hasn&#8217;t necessarily been well received. Earlier this year, <a href="https://www.searchenginejournal.com/googles-mueller-calls-markdown-for-bots-idea-a-stupid-idea/566598/">Google&#8217;s John Mueller called it &#8220;a stupid idea&#8221;</a> on Bluesky, questioning whether LLM crawlers even recognise a .md file as anything other than a plain text blob. </p>
<p>It&#8217;s a fair concern, and we&#8217;d largely agree with it. Maintaining a parallel, machine-facing version of your site introduces problems around trust and verification (how does a crawler know the markdown matches what a user would actually see?) as well as the ongoing cost of keeping the two versions in sync.</p>
<p><strong>That&#8217;s not what this guide is about though. </strong>We&#8217;re covering the opposite use case: extracting content that already exists on a site into clean markdown, for use in downstream tooling. Think RAG pipelines over your own content, content audits, training data preparation, migration archives or competitor analysis. A one-off or periodic extraction for a specific purpose, not a live feed served to crawlers.</p>
<p>With that out of the way, on to the workflow.</p>
<hr class="light" />
<h2 id="approaches">The Two Approaches</h2>
<p>We&#8217;ve got two different methods to cover, and both make use of the SEO Spider&#8217;s <a href="https://www.screamingfrog.co.uk/seo-spider/user-guide/configuration/#custom-javascript">Custom JavaScript</a> feature. We&#8217;d recommend starting with the first one, as it requires no configuration and works well on the vast majority of sites. If it misses the mark on something, the second method gives you more control over what&#8217;s being extracted.</p>
<ul>
<li><strong>Readability.js + Turndown</strong> – A single Custom JavaScript snippet that works across most sites with no configuration. Start here.</li>
<li><strong>Visual Custom Extraction + Turndown</strong> – A fallback for when Readability gets it wrong, or when you only want to extract a specific part of the page rather than the whole article.</li>
</ul>
<p>Both methods require JavaScript rendering to be enabled in the SEO Spider (Configuration > Spider > Rendering > JavaScript), since the Custom JavaScript feature runs against a fully rendered page.</p>
<hr class="light" />
<h2 id="readability">1. Readability.js + Turndown (The Easy Method)</h2>
<p>This approach uses two JavaScript libraries to do the heavy lifting:</p>
<ul>
<li><strong><a href="https://github.com/mozilla/readability">Readability.js</a></strong> is Mozilla&#8217;s open-source library, and is the same engine that powers Firefox&#8217;s Reader View. It analyses the DOM of a page and uses a scoring algorithm to work out which block of markup is the actual content, discarding everything else.</li>
<li><strong><a href="https://github.com/mixmark-io/turndown">Turndown</a></strong> is a small JavaScript library that converts HTML to markdown.</li>
</ul>
<p>Put them together and you&#8217;ve got a full HTML-to-markdown pipeline that runs within the SEO Spider&#8217;s rendering process. Readability figures out where the content is, and Turndown formats it. There&#8217;s nothing to configure per site, and no maintenance to worry about when a site redesigns.</p>
<h3 id="setting-it-up">Setting It Up</h3>
<p>Head over to Configuration > Custom > Custom JavaScript in the SEO Spider, and click &#8216;Add&#8217; to create a new snippet. Give it a sensible name (&#8216;Page Markdown&#8217; works), and paste in the following:</p>
<pre><code class="copy-content" style="width: 850px; overflow: auto; height: 500px; font-weight: 100;">// Markdown extraction using Mozilla Readability + Turndown
//
// Readability.js automatically identifies the main content area (the same
// tech behind Firefox Reader View), then Turndown converts it to markdown.
//
// No content selector needed - Readability figures it out automatically.
//
// SETUP:
// 1. Enable JavaScript Rendering (Config > Spider > Rendering > JavaScript)
// 2. Crawl - markdown appears in the Custom JavaScript column
//

function loadScripts() {
    return new Promise((resolve, reject) => {
        const readability = document.createElement('script');
        readability.src = 'https://unpkg.com/@mozilla/readability/Readability.js';
        readability.onload = () => {
            const turndown = document.createElement('script');
            turndown.src = 'https://unpkg.com/turndown/dist/turndown.js';
            turndown.onload = () => resolve();
            turndown.onerror = () => reject(new Error('Failed to load Turndown.js'));
            document.head.appendChild(turndown);
        };
        readability.onerror = () => reject(new Error('Failed to load Readability.js'));
        document.head.appendChild(readability);
    });
}

function extractMarkdown() {
    // Readability needs a cloned document as it modifies the DOM
    const documentClone = document.cloneNode(true);
    const article = new Readability(documentClone).parse();

    if (!article || !article.content) {
        return 'Readability could not extract content from this page';
    }

    const turndownService = new TurndownService({
        headingStyle: 'atx',
        codeBlockStyle: 'fenced',
        bulletListMarker: '-'
    });

    // Only keep images with meaningful alt text
    turndownService.addRule('cleanImages', {
        filter: 'img',
        replacement: function(content, node) {
            const alt = node.getAttribute('alt') || '';
            const src = node.getAttribute('src') || '';
            if (!alt.trim()) return '';
            return '![' + alt + '](' + src + ')';
        }
    });

    // Remove empty links (e.g. image wrappers with no content)
    turndownService.addRule('removeEmptyLinks', {
        filter: function(node) {
            return node.nodeName === 'A' && !node.textContent.trim();
        },
        replacement: function() {
            return '';
        }
    });

    const markdown = turndownService.turndown(article.content);

    // Build frontmatter from Readability metadata
    const frontmatter = [
        '---',
        article.title ? 'title: "' + article.title.replace(/"/g, '\\"') + '"' : null,
        article.byline ? 'author: "' + article.byline.replace(/"/g, '\\"') + '"' : null,
        article.siteName ? 'site: "' + article.siteName.replace(/"/g, '\\"') + '"' : null,
        article.excerpt ? 'excerpt: "' + article.excerpt.replace(/"/g, '\\"') + '"' : null,
        '---'
    ].filter(Boolean).join('\n');

    return frontmatter + '\n\n' + markdown.replace(/\n{3,}/g, '\n\n').trim();
}

return loadScripts()
    .then(() => seoSpider.data(extractMarkdown()))
    .catch(error => seoSpider.error(error));
</code></pre>
<p>There&#8217;s a bit more going on here than just the core Readability-and-Turndown steps, so it&#8217;s worth briefly walking through what the snippet does:</p>
<ul>
<li><strong>Loads the two libraries from a CDN.</strong> Readability.js and Turndown are injected into the page at crawl time, so there&#8217;s nothing to host or install.</li>
<li><strong>Runs Readability against a clone of the page.</strong> Readability modifies the DOM as it scores elements, so we clone first to avoid affecting the rest of the snippet.</li>
<li><strong>Adds a couple of Turndown rules.</strong> Images without meaningful alt text are dropped (they rarely add value to an LLM), and empty link wrappers are removed. These are small tweaks that keep the output cleaner.</li>
<li><strong>Builds a YAML frontmatter block</strong> using the metadata Readability extracts, including the page title, author/byline, site name and excerpt. This is handy downstream, as most tools that consume markdown will parse frontmatter automatically.</li>
<li><strong>Returns the result via <span class="code">seoSpider.data()</span></strong>, which writes it out to a dedicated column in the crawl data. Any errors are caught and passed through <span class="code">seoSpider.error()</span> so failures show up clearly in the crawl.</li>
</ul>
<h3 id="running-the-crawl">Running the Crawl</h3>
<p>With JavaScript rendering enabled and the snippet saved, go ahead and start your crawl as normal. Once it&#8217;s underway, click over to the Custom JavaScript tab and you&#8217;ll see a new column for the snippet, containing the generated markdown for each page.</p>
<img alt="" class="aligncenter size-full wp-image-360952" height="692" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/md-output.png" width="1764" />
<p>A few things to be aware of:</p>
<ul>
<li><strong>Readability needs enough content to work with.</strong> Pages like login forms, thin category stubs or contact pages will often return nothing, as there&#8217;s no article to actually extract. This is usually the correct behaviour.</li>
<li><strong>Non-content elements are dropped automatically.</strong> Navigation, related posts widgets, share buttons and similar bits are scored low and ignored without needing to be told. It&#8217;s one of the main reasons Readability works so well across different site templates.</li>
<li><strong>JavaScript rendering is slower than standard crawling.</strong> This is a limitation of JS rendering itself, not the snippet. For larger sites, consider crawling in segments or adjusting your rendering settings to suit.</li>
</ul>
<p>If you&#8217;re working with a clean URL structure, consider leveraging the <a href="https://www.screamingfrog.co.uk/seo-spider/user-guide/configuration/#include">Include</a> feature, for example:</p>
<pre><code style="font-weight: 100;">https://www.screamingfrog.co.uk/blog/
\.js</code></pre>
<p>This would just crawl all the content on the Screaming Frog blog. </p>
<p>Once the crawl finishes, export either the Custom JavaScript tab or the Internal > All tab as CSV and you&#8217;re ready for the next step.</p>
<hr class="light" />
<h2 id="visual-extraction">2. Visual Custom Extraction + Turndown (For More Control)</h2>
<p>Readability is good, but it isn&#8217;t perfect. On sites with unusual layouts, for example documentation hubs with heavy sidebars, e-commerce PDPs with lots of supporting content, or certain single-page app setups, it can occasionally grab the wrong section or miss part of the content. It also isn&#8217;t much help if you only want a specific part of the page, such as the product description, the review section or the Q&amp;A block.</p>
<p>For these scenarios, you can tell the SEO Spider exactly where to look using <a href="https://www.screamingfrog.co.uk/seo-spider/user-guide/configuration/#custom-extraction">Custom Extraction</a>, then feed that section through Turndown.</p>
<h3 id="finding-selector">Finding the Right Selector</h3>
<p>Visual Custom Extraction lets you click on a rendered page inside the SEO Spider, and it&#8217;ll automatically generate a CSS selector or Xpath for whatever you clicked on. It&#8217;s by far the easiest way to get a reliable selector without having to dig around in browser dev tools.</p>
<ol>
<li>Go to Configuration > Custom > Custom Extraction and click &#8216;Add&#8217;.</li>
<li>Select &#8216;Visual Extractor&#8217; (Globe icon) and enter a representative URL. We&#8217;d recommend a typical article or product page rather than the homepage, as we want a template that contains the main content area.</li>
<li>When the page loads, click on the main content of the page. The SEO Spider will automatically generate a CSS selector.</li>
<li>Check the preview on the right. If it&#8217;s picking up the sidebar, author bio or other bits you don&#8217;t want, try clicking a more specific element instead.</li>
<li>Set the extraction type to &#8216;Inner HTML&#8217;.</li>
</ol>
<img alt="" class="aligncenter size-full wp-image-360957" height="899" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/visual-custom-extraction.png" width="1310" />
<p>Of course, if you&#8217;re well-versed in CSS Selectors and Xpath, you can likely skip this step and just get what you need from the browser Inspect pane.</p>
<p>Once you&#8217;re happy with what&#8217;s being extracted, copy the selector. We&#8217;ll drop it straight into the JS snippet below.</p>
<h3 id="the-snippet">The Snippet</h3>
<p>This snippet uses Turndown, but not Readability. Instead, it takes the CSS selector you have, pulls the inner HTML, strips out a list of elements that tend not to be part of the main content, and then feeds the rest into Turndown.</p>
<pre><code class="copy-content" style="width: 850px; overflow: auto; height: 500px; font-weight: 100;">// Markdown extraction using Visual Custom Extraction + Turndown
//
// Use this when you want precise control over what content gets extracted.
//
// SETUP:
// 1. Use Visual Custom Extraction (CSS Selector mode) to click your content area
// 2. Copy the CSS selector SF generates and paste it below as CONTENT_SELECTOR
// 3. Test on a few pages - if unwanted elements appear, add their selectors
//    to STRIP_SELECTORS below
// 4. Enable JavaScript Rendering (Config > Spider > Rendering > JavaScript)
// 5. Crawl - markdown appears in the Custom JavaScript column
//

// Paste your CSS selector from Visual Custom Extraction here
const CONTENT_SELECTOR = "YOUR_CSS_SELECTOR_HERE";

// Add any site-specific elements you want to strip out
// These are common defaults - inspect your pages and add to this list as needed
const STRIP_SELECTORS = [
    'nav', 'footer', 'header', 'aside',
    'script', 'style', 'noscript', 'iframe', 'svg',
    '.sidebar', '.navigation', '.menu',
    '.cookie-banner', '.social-share', '.comments',
    '.related-posts', '.breadcrumb', '.share-buttons'
    // Add your own below, e.g.:
    // '.author-bio',
    // '.post-meta',
    // '#newsletter-signup'
];

function loadTurndown() {
    return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = 'https://unpkg.com/turndown/dist/turndown.js';
        script.onload = () => resolve();
        script.onerror = () => reject(new Error('Failed to load Turndown.js'));
        document.head.appendChild(script);
    });
}

function extractMarkdown() {
    const content = document.querySelector(CONTENT_SELECTOR);
    if (!content) {
        return 'No content found for selector: ' + CONTENT_SELECTOR;
    }

    // Clone so we don't modify the live DOM
    const clone = content.cloneNode(true);

    // Strip non-content elements
    STRIP_SELECTORS.forEach(selector => {
        clone.querySelectorAll(selector).forEach(el => el.remove());
    });

    const turndownService = new TurndownService({
        headingStyle: 'atx',
        codeBlockStyle: 'fenced',
        bulletListMarker: '-'
    });

    // Only keep images with meaningful alt text
    turndownService.addRule('cleanImages', {
        filter: 'img',
        replacement: function(content, node) {
            const alt = node.getAttribute('alt') || '';
            const src = node.getAttribute('src') || '';
            if (!alt.trim()) return '';
            return '![' + alt + '](' + src + ')';
        }
    });

    // Remove empty links (e.g. image wrappers with no content)
    turndownService.addRule('removeEmptyLinks', {
        filter: function(node) {
            return node.nodeName === 'A' && !node.textContent.trim();
        },
        replacement: function() {
            return '';
        }
    });

    const markdown = turndownService.turndown(clone.innerHTML);

    // Clean up excessive blank lines
    return markdown.replace(/\n{3,}/g, '\n\n').trim();
}

return loadTurndown()
    .then(() => seoSpider.data(extractMarkdown()))
    .catch(error => seoSpider.error(error));
</code></pre>
<p>Two things to edit each time you use this:</p>
<ul>
<li><span class="code">CONTENT_SELECTOR</span> – paste in whatever selector Visual Custom Extraction gave you.</li>
<li><span class="code">STRIP_SELECTORS</span> – the defaults cover the usual suspects, but every site has its own quirks. It&#8217;s worth spending a minute looking at a rendered page and adding anything obvious, like a floating &#8216;book a demo&#8217; bar, an author bio box, or an in-article email capture form.</li>
</ul>
<h3 id="when-to-use-this">When to Use This Instead</h3>
<p>The trade-off here is setup time versus precision. You&#8217;re configuring a selector (and possibly a few strip selectors) per site, but in return you know exactly what&#8217;s being extracted. It&#8217;s usually the right choice when:</p>
<ul>
<li><strong>Readability is getting it wrong.</strong> Run the first method, spot-check a few outputs, and if several pages are coming back with the wrong content, switch over.</li>
<li><strong>You only want a specific section.</strong> For instance, the review content on a product page, or the Q&amp;A block on a FAQ page.</li>
<li><strong>The site has a very consistent template.</strong> A large blog or documentation site where the content always sits in the same wrapper is where a selector-based approach shines.</li>
</ul>
<hr class="light" />
<h2 id="python-export">3. Bulk Exporting Markdown Files With Python</h2>
<p>Once the crawl has finished, export the Custom JavaScript tab from the SEO Spider as an .xlsx file. This gives you every URL alongside its corresponding markdown content, which is fine for a quick look, but most downstream tools like vector databases, fine-tuning pipelines and static site generators will want one file per page rather than one big spreadsheet.</p>
<p>The following Python script handles that. It reads the .xlsx export, takes the URL and markdown content from each row, and saves each page as an individual .md file. Filenames are built from the URL path, so something like <span class="code">screamingfrog.co.uk/blog/design-trends-shaping-2026/</span> becomes <span class="code">blog__design-trends-shaping-2026.md</span>. The source URL is added as an HTML comment at the top of each file for reference.</p>
<pre><code class="copy-content" style="width: 850px; overflow: auto; height: 500px; font-weight: 100;">import pandas as pd
import os
import re
from urllib.parse import urlparse

# Path to your Screaming Frog export
EXPORT_FILE = 'custom_javascript_md_.xlsx'

# Output directory for markdown files
OUTPUT_DIR = 'markdown_output'


def url_to_filename(url):
    """Convert a URL to a filename. Slashes become double underscores."""
    parsed = urlparse(url)
    path = parsed.path.strip('/')
    if not path:
        return 'index.md'
    filename = path.replace('/', '__')
    filename = re.sub(r'[^\w\-.]', '_', filename)
    filename = re.sub(r'\.(html?|php|aspx?)$', '', filename)
    return filename + '.md'


def main():
    os.makedirs(OUTPUT_DIR, exist_ok=True)

    df = pd.read_excel(EXPORT_FILE)

    # First column is always Address, markdown content is always column D onwards
    url_col = df.columns[0]
    md_col = df.columns[3]

    count = 0
    skipped = 0

    for _, row in df.iterrows():
        url = str(row[url_col]).strip()
        markdown = str(row[md_col]).strip()

        if not markdown or markdown == 'nan' or not url:
            skipped += 1
            continue

        filename = url_to_filename(url)
        filepath = os.path.join(OUTPUT_DIR, filename)

        with open(filepath, 'w', encoding='utf-8') as f:
            f.write(f'&lt;!-- Source: {url} --&gt;\n\n{markdown}')

        count += 1

    print(f'Created {count} markdown files in /{OUTPUT_DIR}')
    if skipped:
        print(f'Skipped {skipped} rows (empty content or URL)')


if __name__ == '__main__':
    main()
</code></pre>
<p>To use it:</p>
<ol>
<li>Install Python if you don&#8217;t already have it, available from <a href="https://www.python.org/">python.org</a>.</li>
<li>Install the two required dependencies with <span class="code">pip install pandas openpyxl</span>.</li>
<li>Save the script as something like <span class="code">export_md.py</span>.</li>
<li>Put it in the same folder as your SF export.</li>
<li>Update the filename on line 6 to match the name of your export.</li>
<li>Run it with <span class="code">python export_md.py</span>.</li>
</ol>
<p>You&#8217;ll end up with a <span class="code">markdown_output</span> folder containing all your .md files, ready to feed into whatever you&#8217;re using them for. LangChain and LlamaIndex document loaders will read them directly, most vector database ingestion tools accept a directory of markdown, and if you just want to grep through the content, tools like <span class="code">ripgrep</span> work fine against the output.</p>
<img alt="" class="aligncenter size-full wp-image-360963" height="712" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/mdexportexample.png" width="1489" />
<p>A couple of things worth being aware of:</p>
<ul>
<li><strong>Empty rows are skipped.</strong> If Readability couldn&#8217;t extract content from a particular page, that row is skipped rather than writing out an empty file. The script will also print the total number of rows it skipped when it finishes.</li>
<li><strong>Column positions are hard-coded.</strong> The script assumes the URL is in the first column and the markdown content in column D, which matches the default SF Custom JavaScript export. If you&#8217;ve added other Custom JavaScript snippets or extractions that shift things around, adjust the <span class="code">md_col</span> line to match.</li>
</ul>
<hr class="light" />
<h2 id="final-thoughts">Final Thoughts</h2>
<p>That covers the full workflow. One crawl, one export, one folder of markdown files ready for whatever you&#8217;re feeding them into. For most sites, the first approach (Readability.js + Turndown) will give you a solid result with no configuration whatsoever. Where it falls short, the second approach gives you the control to pinpoint exactly what gets extracted.</p>
<p>The same pipeline isn&#8217;t just useful for LLM-related tasks. Content audits, migration preparation, competitor analysis and keeping a clean text archive of a site are all valid use cases. Once your Custom JavaScript snippets are saved in the SEO Spider, running the same process on a new site takes a matter of minutes.</p>
<p>If you come up with any useful additions or tweaks to the snippets (particularly the <span class="code">STRIP_SELECTORS</span> array for the second method), let us know in the comments.</p>
<p>The post <a href="https://www.screamingfrog.co.uk/blog/generate-markdown-at-scale/">Using the Screaming Frog SEO Spider to Generate Markdown at Scale</a> appeared first on <a href="https://www.screamingfrog.co.uk">Screaming Frog</a>.</p>
