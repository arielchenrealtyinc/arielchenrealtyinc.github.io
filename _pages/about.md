---
layout: page
title: Ariel Chen
permalink: /about/
image: '/assets/images/ariel.jpg'
---

# About Ariel

Ariel is a top real estate agent in the San Francisco Bay Area.
She believes that buying/selling a home is one of the biggest decisions for families.
She consistently puts her clients on her top priority. She always think it as her greatest accomplishment to use her excellent negotiation ability and professional house market knowledge to gain the best result for her clients. Her clients all speak highly of her work and appreciate her as a detail-oriented and customized-servicing agent. If you have any question about buying/selling/renting houses, getting a loan, etc. Please do not hesitate to give her a call right away.

# Ariel's Story

<iframe src="https://www.youtube.com/embed/UsBAmZY8V9k" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br/>

# Contact Ariel

{% if site.contact.formspree_id %}
<div class="form-box">
  <div class="contact-head"></div>
  <form class="form" action="https://formspree.io/f/{{site.contact.formspree_id}}" method="POST">
    <div class="form__group">
      <label class="form__label screen-reader-text" for="form-name">Your Name</label>
      <input class="form__input" id="form-name" type="text" name="name" placeholder="Name..." required>
    </div>
    <div class="form__group">
      <label class="form__label screen-reader-text" for="form-email">Your Email</label>
      <input class="form__input" id="form-email" type="email" name="_replyto" placeholder="Email..." required>
    </div>
    <div class="form__group">
      <label class="form__label screen-reader-text" for="form-text">Your Message</label>
      <textarea class="form__input" id="form-text" name="text" rows="10" placeholder="Message..." required></textarea>
    </div>
    <div class="form__group">
      <button class="button button--rounded" type="submit">Send</button>
    </div>
  </form>
</div>
{% endif %}