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
> Feeds update automatically when you load the page. Visit our [RSS Feeds](/feeds/) page to subscribe directly to these sources. Click category headings to expand/collapse sections.

---

<div id="feed-loading" style="text-align: center; padding: 2rem;">
  <p>⏳ Loading latest news from RSS feeds...</p>
</div>

<div id="rss-feeds" style="display: none;">

  <details open class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">🔒 Security News</h2>
      <span class="item-count" id="security-count"></span>
    </summary>
    <div id="security-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

  <details open class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">🤖 AI & ML News</h2>
      <span class="item-count" id="ai-count"></span>
    </summary>
    <div id="ai-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

  <details open class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">💻 Tech News</h2>
      <span class="item-count" id="tech-count"></span>
    </summary>
    <div id="tech-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

  <details class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">🛡️ CVE Alerts</h2>
      <span class="item-count" id="cve-count"></span>
    </summary>
    <div id="cve-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

  <details class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">☁️ DevOps News</h2>
      <span class="item-count" id="devops-count"></span>
    </summary>
    <div id="devops-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

  <details class="feed-section">
    <summary class="feed-section-header">
      <h2 style="display: inline;">👨‍💻 Programming News</h2>
      <span class="item-count" id="programming-count"></span>
    </summary>
    <div id="programming-feeds" class="feed-category">
      <div class="feed-items"></div>
    </div>
  </details>

</div>

<script>
// RSS Feed Configuration - Top 2 from each feed source
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
    'https://nvd.nist.gov/feeds/xml/cve/misc/nvd-rss.xml',
    'https://www.bleepingcomputer.com/feed/'
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

// Fetch and display feeds using RSS2JSON API (FREE TIER - NO COUNT PARAMETER)
async function loadFeeds(category, feedUrls, itemsPerFeed = 2) {
  const container = document.querySelector(`#${category}-feeds .feed-items`);
  const countElement = document.getElementById(`${category}-count`);
  const maxTotalItems = 6; // Maximum items to display per category
  let totalItems = 0;

  for (const feedUrl of feedUrls) {
    if (totalItems >= maxTotalItems) break;

    // FIXED: Removed &count parameter that requires paid API key
    const apiUrl = `https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}`;

    try {
      const response = await fetch(apiUrl);
      const data = await response.json();

      if (data.status === 'ok' && data.items && data.items.length > 0) {
        // RSS2JSON returns up to 10 items by default - slice to get top 2 per feed
        const itemsToShow = Math.min(itemsPerFeed, maxTotalItems - totalItems);

        data.items.slice(0, itemsToShow).forEach(item => {
          const feedItem = document.createElement('div');
          feedItem.className = 'feed-item';

          // Clean up description
          let description = item.description || item.content || '';
          description = description.replace(/<[^>]*>/g, ''); // Strip HTML
          description = description.replace(/&[^;]+;/g, ' '); // Remove HTML entities
          description = description.trim().substring(0, 200);

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
      } else if (data.status === 'error') {
        console.error(`API Error for ${feedUrl}:`, data.message);
      }
    } catch (error) {
      console.error(`Error fetching ${feedUrl}:`, error);
    }
  }

  // Update item count
  if (countElement) {
    if (totalItems > 0) {
      countElement.textContent = `(${totalItems} items)`;
      countElement.style.color = '#28a745';
    } else {
      countElement.textContent = '(loading...)';
      countElement.style.color = '#6c757d';
    }
  }

  if (totalItems === 0) {
    container.innerHTML = '<p><em>No items available at this time. Please check console for errors.</em></p>';
  }

  return totalItems;
}

// Load all feeds with progress indication
async function loadAllFeeds() {
  const loadingDiv = document.getElementById('feed-loading');
  const feedsDiv = document.getElementById('rss-feeds');

  try {
    // Show the feeds div first so users see loading progress per category
    feedsDiv.style.display = 'block';
    loadingDiv.style.display = 'none';

    // Load feeds one category at a time to show progressive loading
    await loadFeeds('security', feedConfig.security, 2);
    await loadFeeds('ai', feedConfig.ai, 2);
    await loadFeeds('tech', feedConfig.tech, 2);
    await loadFeeds('cve', feedConfig.cve, 3);
    await loadFeeds('devops', feedConfig.devops, 2);
    await loadFeeds('programming', feedConfig.programming, 2);

  } catch (error) {
    console.error('Error loading feeds:', error);
    loadingDiv.innerHTML = '<p style="color: red;">Error loading feeds. Please check console and refresh the page.</p>';
    loadingDiv.style.display = 'block';
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
/* Collapsible section styling */
.feed-section {
  margin: 1.5rem 0;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  padding: 0.5rem;
  background: #ffffff;
}

.feed-section-header {
  cursor: pointer;
  padding: 0.75rem;
  user-select: none;
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.feed-section-header:hover {
  background: #f6f8fa;
  border-radius: 4px;
}

.feed-section-header h2 {
  margin: 0;
  font-size: 1.5rem;
}

.item-count {
  font-size: 0.9rem;
  color: #6c757d;
  font-weight: normal;
}

/* Remove default details arrow and add custom one */
.feed-section summary {
  list-style: none;
}

.feed-section summary::-webkit-details-marker {
  display: none;
}

.feed-section summary::before {
  content: '▶';
  display: inline-block;
  margin-right: 0.5rem;
  transition: transform 0.2s;
}

.feed-section[open] summary::before {
  transform: rotate(90deg);
}

.feed-category {
  margin: 1rem 0 0.5rem 0;
  padding: 0 0.75rem;
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

  .feed-section-header h2 {
    font-size: 1.25rem;
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
