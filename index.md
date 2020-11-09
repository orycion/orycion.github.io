---
layout: home
---

<div class="separator">Products</div>

<div style="column-count: 3;">
  {% for product in site.products %}
    <a href="{{ product.website }}">
      <img src="/assets/products/{{ product.icon }}.png" alt="" />
      <p style="text-align: center;">{{ product.name }}</p>
    </a>
  {% endfor %}
</div>

<!-- <div class="separator">Values</div>

<dl>
  <dt>Open</dt>
  <dd>We’re always receptive to feedback and strive to be honest in everything we do.</dd>

  <dt>Quality</dt>
  <dd>We always aim to provide a high quality experience to our customers. There’s nothing we hate more than bad quality software.</dd>

  <dt>Educate</dt>
  <dd>A sea serpent.</dd>
</dl> -->
