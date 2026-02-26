---
layout: default
title: Search the Archive
permalink: /search/
description: Search through the complete archive of the D Language Blog.
---

<div class="search-container" style="max-width: 800px; margin: 0 auto; padding: 20px;">
  <h1>Search Results</h1>
  
  <div style="display: flex; gap: 10px; margin-bottom: 20px;">
    <input type="text" id="search-input" placeholder="Search by title, tag, or year..." 
           style="flex-grow: 1; padding: 10px; font-size: 1rem; border: 1px solid #ddd; border-radius: 4px;">
    <button id="search-button" style="padding: 10px 20px; background: #b03931; color: white; border: none; border-radius: 4px; cursor: pointer;">
      Search
    </button>
  </div>

  <div id="search-status" style="margin-bottom: 20px; font-style: italic; color: #666;">Initializing search...</div>
  
  <ul id="results-list" style="list-style: none; padding: 0;"></ul>
</div>

<script>
async function initSearch() {
  const input = document.getElementById('search-input');
  const list = document.getElementById('results-list');
  const status = document.getElementById('search-status');
  const btn = document.getElementById('search-button');

  try {
    const response = await fetch("{{ '/search.json' | relative_url }}");
    const posts = await response.json();

    const performSearch = (queryOverride) => {
      const query = (queryOverride || input.value).toLowerCase().trim();
      input.value = query; // Sync input box with URL param if needed
      list.innerHTML = '';

      if (query.length < 2) {
        status.textContent = "Please enter at least 2 characters.";
        return;
      }

      const results = posts.filter(post => 
        post.title.toLowerCase().includes(query) || 
        (post.tags && post.tags.some(t => t.toLowerCase().includes(query))) ||
        post.date.toLowerCase().includes(query)
      );

      if (results.length === 0) {
        status.textContent = `No results found for "${query}"`;
      } else {
        status.textContent = `Found ${results.length} article(s):`;
        results.forEach(post => {
          const li = document.createElement('li');
          li.style.padding = "10px 0";
          li.style.borderBottom = "1px solid #eee";
          li.innerHTML = `<a href="${post.url}" style="color: #b03931; font-weight: bold; text-decoration: none;">${post.title}</a><br><small>${post.date}</small>`;
          list.appendChild(li);
        });
      }
    };

    // 1. Check for URL parameters (from your sidebar form)
    const urlParams = new URLSearchParams(window.location.search);
    const initialQuery = urlParams.get('q');
    if (initialQuery) {
      performSearch(initialQuery);
    } else {
      status.textContent = "";
    }

    // 2. Event Listeners for on-page search
    input.addEventListener('input', () => performSearch());
    btn.addEventListener('click', () => performSearch());
    input.addEventListener('keypress', (e) => { if (e.key === 'Enter') performSearch(); });

  } catch (e) {
    status.textContent = "Error loading search index.";
    console.error(e);
  }
}
document.addEventListener('DOMContentLoaded', initSearch);
</script>