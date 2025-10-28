---
layout: default
title: Home
nav_order: 1
description: "Tech news aggregator - Latest updates in Security, AI, DevOps, and more"
permalink: /
---

# Welcome to My Tech News Hub

Stay updated with the latest news from curated RSS feeds across multiple technology domains.

{: .note }
> Feeds update automatically when you load the page. Visit our [RSS Feeds](/feeds/) page to subscribe directly to these sources.

---

<div id="feed-loading" style="text-align: center; padding: 2rem;">
  <p>⏳ Loading latest news from RSS feeds...</p>
</div>

<div id="rss-feeds" style="display: none;">

  <h2>🔒 Latest Security News</h2>
  <div id="security-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

  <h2>🤖 Latest AI & ML News</h2>
  <div id="ai-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

  <h2>💻 Latest Tech News</h2>
  <div id="tech-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

  <h2>🛡️ Latest CVE Alerts</h2>
  <div id="cve-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

  <h2>☁️ Latest DevOps News</h2>
  <div id="devops-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

  <h2>👨‍💻 Latest Programming News</h2>
  <div id="programming-feeds" class="feed-category">
    <div class="feed-items"></div>
  </div>

</div>

<script>
// RSS Feed Configuration - Top 2 from each category
const feedConfig = {
  security: [
    'https://krebsonsecurity.com/feed/',
    'https://www.schneier.com/blog/atom.xml',
    'https://feeds.feedburner.com/TheHackersNews'
  ],
  ai: [
    'https://openai.com/blog/rss/',
    'https://blog.research.google/feeds/posts/default',
    'https://blogs.microsoft.com/ai/feed/'
  ],
  tech: [
    'https://techcrunch.com/feed/',
    'https://www.theverge.com/rss/index.xml',
    'https://feeds.arstechnica.com/arstechnica/index'
  ],
  cve: [
    'https://cvefeed.io/rssfeed-critical.xml',
    'https://www.cisa.gov/cybersecurity-advisories/all.xml'
  ],
  devops: [
    'https://devops.com/feed/',
    'https://aws.amazon.com/blogs/devops/feed/',
    'https://kubernetes.io/feed.xml'
  ],
  programming: [
    'https://news.ycombinator.com/rss',
    'https://github.blog/feed/',
    'https://stackoverflow.blog/feed/'
  ]
};

// Fetch and display feeds using RSS2JSON API
async function loadFeeds(category, feedUrls, itemsPerFeed = 2) {
  const container = document.querySelector(`#${category}-feeds .feed-items`);
  const maxTotalItems = 6; // Maximum items to display per category
  let totalItems = 0;

  for (const feedUrl of feedUrls) {
    if (totalItems >= maxTotalItems) break;

    const apiUrl = `https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}&count=${itemsPerFeed}`;

    try {
      const response = await fetch(apiUrl);
      const data = await response.json();

      if (data.status === 'ok' && data.items) {
        const itemsToShow = Math.min(itemsPerFeed, maxTotalItems - totalItems);

        data.items.slice(0, itemsToShow).forEach(item => {
          const feedItem = document.createElement('div');
          feedItem.className = 'feed-item';

          // Clean up description
          let description = item.description || item.content || '';
          description = description.replace(/<[^>]*>/g, ''); // Strip HTML
          description = description.substring(0, 200);

          // Format date
          const pubDate = item.pubDate ? new Date(item.pubDate).toLocaleDateString('en-US', {
            year: 'numeric',
            month: 'short',
            day: 'numeric'
          }) : '';

          feedItem.innerHTML = `
            <h4><a href="${item.link}" target="_blank" rel="noopener">${item.title}</a></h4>
            <p class="feed-meta">
              <small>${pubDate} • ${data.feed.title}</small>
            </p>
            ${description ? `<p class="feed-description">${description}...</p>` : ''}
          `;
          container.appendChild(feedItem);
          totalItems++;
        });
      }
    } catch (error) {
      console.error(`Error fetching ${feedUrl}:`, error);
    }
  }

  if (totalItems === 0) {
    container.innerHTML = '<p><em>No items available at this time.</em></p>';
  }
}

// Load all feeds
async function loadAllFeeds() {
  const loadingDiv = document.getElementById('feed-loading');
  const feedsDiv = document.getElementById('rss-feeds');

  try {
    await Promise.all([
      loadFeeds('security', feedConfig.security, 2),
      loadFeeds('ai', feedConfig.ai, 2),
      loadFeeds('tech', feedConfig.tech, 2),
      loadFeeds('cve', feedConfig.cve, 3),
      loadFeeds('devops', feedConfig.devops, 2),
      loadFeeds('programming', feedConfig.programming, 2)
    ]);

    loadingDiv.style.display = 'none';
    feedsDiv.style.display = 'block';
  } catch (error) {
    loadingDiv.innerHTML = '<p style="color: red;">Error loading feeds. Please refresh the page.</p>';
  }
}

// Initialize on page load
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', loadAllFeeds);
} else {
  loadAllFeeds();
}
</script>

<style>
.feed-category {
  margin: 1.5rem 0 2.5rem 0;
}

.feed-items {
  display: grid;
  gap: 1rem;
}

.feed-item {
  padding: 1rem;
  background: #f6f8fa;
  border-left: 3px solid #0366d6;
  border-radius: 4px;
  transition: transform 0.2s, box-shadow 0.2s;
}

.feed-item:hover {
  transform: translateX(4px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.feed-item h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1.1rem;
  line-height: 1.4;
}

.feed-item h4 a {
  color: #0366d6;
  text-decoration: none;
}

.feed-item h4 a:hover {
  text-decoration: underline;
}

.feed-meta {
  color: #586069;
  margin: 0.5rem 0;
  font-size: 0.9rem;
}

.feed-description {
  color: #24292e;
  font-size: 0.95rem;
  line-height: 1.5;
  margin: 0.5rem 0 0 0;
}

@media (max-width: 768px) {
  .feed-item {
    padding: 0.75rem;
  }

  .feed-item h4 {
    font-size: 1rem;
  }
}
</style>

---

## More Resources

- **[View All RSS Feeds](/feeds/)** - Browse our complete collection of 60+ curated technology RSS feeds
- **[About This Site](/about/)** - Learn more about this tech news aggregator

---

{: .fs-3 }
This site is built with Jekyll, Just the Docs theme, and hosted on GitHub Pages.
