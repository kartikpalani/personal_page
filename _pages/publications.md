---
layout: page
permalink: /publications/
title: publications
description: More details can be found on <a href="https://scholar.google.com/citations?user=I2kASBIAAAAJ">google scholar</a>.
years: [2019,2018,2017,2016,2015,2014]
---
<h3 class="year">Preprints</h3>
{% bibliography -f preprints %}

{% for y in page.years %}
  <h3 class="year">{{y}}</h3>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
