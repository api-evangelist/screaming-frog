---
title: "Screaming Frog Log File Analyser Update – Version 7.0"
url: "https://www.screamingfrog.co.uk/blog/log-file-analyser-7-0/"
date: "Wed, 29 Apr 2026 10:41:56 +0000"
author: "screamingfrog"
feed_url: "https://www.screamingfrog.co.uk/feed/"
---
<p>We&#8217;re delighted to announce the release of the <strong>Screaming <span style="color: #7ac71f;">Frog</span></strong> Log File Analyser 7.0.</p>
<p>If you&#8217;re not already familiar with the <a href="https://www.screamingfrog.co.uk/log-file-analyser/">Log File Analyser</a>, it allows you to upload raw server log files, verify bots, and get valuable insight into both search engine and AI bots crawling your website.</p>
<img alt="Screaming Frog Log File Analyser 7.0" class="alignnone size-full wp-image-362009" height="1366" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/screaming-frog-log-file-analyser-7.png" width="2557" />
<p>In this release we have a number of new features, as well as plenty of smaller quality of life improvements. Here&#8217;s what&#8217;s new.</p>
<hr class="light" />
<h2>1) Bot Verification for AI Bots</h2>
<p>AI bots now support bot verification, so you can confirm it is a genuine AI bot crawling your website, and not being spoofed.</p>
<img alt="AI Bot Verification" class="alignnone size-full wp-image-361981" height="817" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/ai-bot-verification.png" width="1300" />
<p>Similar to search bot verificiation, you can just verify bots during the upload process.</p>
<img alt="Verify bots on Import" class="alignnone size-full wp-image-361984" height="665" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/verify-bots-import.png" width="976" />
<p>Or via the &#8216;Project > Verify Bots&#8217; menu after upload.</p>
<img alt="Verify Bots menu" class="alignnone size-full wp-image-361985" height="427" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/verify-bots-menu.png" width="936" />
<p>This opens up the verification status filter for bot types that support it, so you can only view verified genuine bots, or identify spoofed bots.</p>
<img alt="Verification Status" class="alignnone size-full wp-image-361986" height="463" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/verification-status.png" width="955" />
<hr class="light" />
<h2>2) User Agent Grouping</h2>
<p>With so many different bots today, we have added bot groupings to make it easy to filter for the bots you wish to upload and analyse in projects.</p>
<img alt="User-Agent Groupings" class="alignnone size-full wp-image-361989" height="668" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/user-agent-groupings.png" width="979" />
<p>This then allows you to analyse by the same groups using the global user agent filter, such as &#8216;All Search Bots&#8217;, or &#8216;All AI Bots&#8217;.</p>
<img alt="User-agent Groupings filter" class="alignnone size-full wp-image-361990" height="747" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/user-agent-groupings-filter.png" width="830" />
<p>The Overview tab also includes top level statistics of these groupings, so you can quickly see the proporition crawling a site.</p>
<hr class="light" />
<h2>3) Customisable Bot Verification</h2>
<p>When you add a new user agent, you&#8217;re able to configure bot verification. You can input IP ranges from a JSON URL, reverse DNS, ASN or static IP ranges to perform verification.</p>
<img alt="Customisable Bot Verification" class="alignnone size-full wp-image-361993" height="922" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/customisable-bot-verification.png" width="1047" />
<p>If bot verification changes for any existing user agents, you do not need to wait for our team to push an updated version of the Log File Analyser, you can just update immediately.</p>
<hr class="light" />
<h2>4) Discovery of Unknown User Agents</h2>
<p>There&#8217;s a new &#8216;Include unknown User Agents&#8217; setting in the User Agents tab of a new project.</p>
<img alt="Unknown User-agents" class="alignnone size-full wp-image-361995" height="688" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/unknown-user-agents.png" width="1006" />
<p>Enabling this option means any user agents that are not known (defined by your full &#8216;Available&#8217; list of user agents) will be included in the analysis.</p>
<p>This can be really useful for analysing bots that are crawling your website, that are not browsers, or known search or AI bots etc. </p>
<img alt="Unknown User-agents in the user-agents tab" class="alignnone size-full wp-image-361997" height="1271" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/unknown-user-agents-user-agents-tab.png" width="2284" />
<p>These might be bots you wish to add to the full user agent list to monitor more closely, or potentially block.</p>
<hr class="light" />
<h2>5) Import &#038; Export Projects</h2>
<p>You&#8217;re now able to export and import projects. This can be seen under the top level &#8216;Project&#8217; menu.</p>
<img alt="Export and Import projects" class="alignnone size-full wp-image-362000" height="427" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/import-export-projects.png" width="908" />
<p>This allows you to share projects with other users, or archive without having to re-analyse the log files.</p>
<hr class="light" />
<h2>6) Export to Google Sheets</h2>
<p>Alongside exporting as a CSV, or Excel, you&#8217;re now able to export to Google Sheets directly.</p>
<img alt="Export to Google Sheets" class="alignnone size-full wp-image-362002" height="526" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/export-google-sheets.png" width="1130" />
<p>Just add your Google account, authorise the Log File Analyser and export.</p>
<hr class="light" />
<h2>7) New lower &#8216;Time Series&#8217; Charts</h2>
<p>Across various tabs there is a new lower &#8216;Time Series&#8217; tab, which shows (you guessed it) a time series chart of the data. The filter allows you to switch the graph to different views, from Events, to Response Codes, Bytes, Average Response Times etc.</p>
<img alt="Time Series tab" class="alignnone size-full wp-image-362007" height="1369" src="https://www.screamingfrog.co.uk/wp-content/uploads/2026/04/time-series-tab.png" width="2559" />
<p>You&#8217;re able to highlight a single URL individually, or multiple URLs for an aggregated view.</p>
<p>This time series graph can be extremely useful when analysing patterns, such as sudden changes in response codes, or average response times suddenly spiking over a period of time.</p>
<hr class="light" />
<h2>8) Ubuntu and Fedora packages for Arm</h2>
<p>We now have two more packages available for Arm users for both Ubuntu and Fedora.</p>
<p>At least three users will rejoice!</p>
<hr class="light" />
<h2>Other Updates</h2>
<p>Version 7.0 also includes a number of smaller updates and bug fixes, outlined below.</p>
<ul>
<li><strong>Usage Stats</strong> &#8211; There&#8217;s some fun Usage Stats available under &#8216;Help > Usage Stats&#8217; which display the number of files you&#8217;ve imported, number of log lines as well as top user agents and tabs.</li>
<li><strong>Re-run Bot Verification</strong> &#8211; It&#8217;s now possible to re-run bot verification, without starting a new project. Simply hit &#8216;File > Verify Bots&#8230;&#8217; in the menu.</li>
<li><strong>Advanced Table Search</strong> &#8211; There&#8217;s a new advanced table search across all tabs with better filtering options.</li>
<li><strong>Import XML Sitemaps</strong> &#8211; It&#8217;s now possible to import Sitemap files into the Imported URL Data tab to combinie data against log files and see if they are being crawled, or missing.</li>
<li><strong>Start a Crawl via Right Click</strong> &#8211; You can right click and &#8216;Start Crawl in <a href="https://www.screamingfrog.co.uk/seo-spider/">SEO Spider</a>&#8216; for improved efficiency.</li>
<li><strong>Updated Project Dialog</strong> &#8211; The dialog has been updated to include last modified time, and size. Projects are now ordered by last modified.</li>
<li><strong>Customisable Overview Charts</strong> &#8211; Click on the &#8216;Charts&#8217; button on the Overview tab, and you can add additional charts for Average Response Time, Total Co2 and Bytes. This collates data in the details views/graphs when you multi-select in the main view.</li>
<li><strong>Improved Importing</strong> &#8211; There&#8217;s been a number of improvements to make it more likely to be able to import a log file without user adjustment.</li>
<li><strong>More Responsive</strong> &#8211; Loading data, and navigating between tabs or using filters is now much faster!</li>
<li><strong>Java 25</strong> &#8211; One for the purists. Version 7.0 has been updated to Java 25.</li>
</ul>
<p>We hope you like version 7.0!</p>
<p>If you&#8217;re looking for inspiration for log file analysis, then read our guide on <a href="https://www.screamingfrog.co.uk/blog/22-ways-to-analyse-log-files/">22 ways to analyse log files for SEO</a>.</p>
<p>Once again, thanks to everyone for their continued support, feedback and feature requests. Please let us know if you experience any issues with version 7.0 of the <a href="https://www.screamingfrog.co.uk/log-file-analyser/">Log File Analyser</a> via our <a href="https://www.screamingfrog.co.uk/log-file-analyser/support/">support</a>.</p>
<p>The post <a href="https://www.screamingfrog.co.uk/blog/log-file-analyser-7-0/">Screaming Frog Log File Analyser Update – Version 7.0</a> appeared first on <a href="https://www.screamingfrog.co.uk">Screaming Frog</a>.</p>
