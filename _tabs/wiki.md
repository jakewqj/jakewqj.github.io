---
title: Wiki
icon: fas fa-book
order: 6
layout: page
---

{% include lang.html %}

<h1 class="dynamic-title">China Indie Wiki</h1>

<div class="row g-4 mt-2">
  <div class="col-md-4">
    <a href="{{ '/wiki/labels/' | relative_url }}" class="text-decoration-none">
      <div class="card wiki-entry-card h-100">
        <div class="card-body text-center py-4">
          <i class="fas fa-compact-disc fa-3x mb-3 text-primary"></i>
          <h4 class="card-title">厂牌大全</h4>
          <p class="card-text text-muted small">Labels Directory</p>
          <p class="card-text text-muted small">{{ site.labels.size }} labels</p>
        </div>
      </div>
    </a>
  </div>

  <div class="col-md-4">
    <a href="{{ '/wiki/bands/' | relative_url }}" class="text-decoration-none">
      <div class="card wiki-entry-card h-100">
        <div class="card-body text-center py-4">
          <i class="fas fa-users fa-3x mb-3 text-primary"></i>
          <h4 class="card-title">乐队大全</h4>
          <p class="card-text text-muted small">Bands & Artists Directory</p>
          <p class="card-text text-muted small">{{ site.bands.size }} artists</p>
        </div>
      </div>
    </a>
  </div>

  <div class="col-md-4">
    <a href="{{ '/wiki/venues/' | relative_url }}" class="text-decoration-none">
      <div class="card wiki-entry-card h-100">
        <div class="card-body text-center py-4">
          <i class="fas fa-map-marker-alt fa-3x mb-3 text-primary"></i>
          <h4 class="card-title">场地大全</h4>
          <p class="card-text text-muted small">Venues Directory</p>
          <p class="card-text text-muted small">{{ site.venues.size }} venues</p>
        </div>
      </div>
    </a>
  </div>
</div>
