---
layout: page
permalink: /publications/
title: publications
description: publications by categories in reversed chronological order.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

<!-- {% include bib_search.liquid %} -->

<div class="publications">

{% if site.display_topics %}
  <div class="filter-bar">
    <div class="filter-bar-label">Filter by topic</div>
    <div class="filter-pills">
      <button type="button" class="filter-pill color-all active" data-filter="all">All</button>
      {% for topic in site.display_topics %}
        {% assign color_index = forloop.index0 | modulo: 7 %}
        <button type="button" class="filter-pill color-{{ color_index }}" data-filter="{{ topic.slug }}">
          {{ topic.label }}
        </button>
      {% endfor %}
    </div>
  </div>
{% endif %}

{% bibliography%}


</div>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    var filterBtns = document.querySelectorAll('.publications .filter-pill');
    var groups = document.querySelectorAll('.publications ol.bibliography');
    if (!filterBtns.length || !groups.length) return;

    filterBtns.forEach(function (btn) {
      btn.addEventListener('click', function () {
        var filter = btn.getAttribute('data-filter');

        filterBtns.forEach(function (b) {
          b.classList.toggle('active', b.getAttribute('data-filter') === filter);
        });

        groups.forEach(function (ol) {
          var visibleCount = 0;

          Array.prototype.forEach.call(ol.children, function (li) {
            var row = li.querySelector('[data-topics]');
            var topics = row ? row.getAttribute('data-topics').split(',').filter(Boolean) : [];
            var match = filter === 'all' || topics.indexOf(filter) !== -1;
            li.hidden = !match;
            if (match) visibleCount++;
          });

          ol.hidden = visibleCount === 0;
          var heading = ol.previousElementSibling;
          if (heading && heading.classList.contains('bibliography')) {
            heading.hidden = visibleCount === 0;
          }
        });
      });
    });
  });
</script>
