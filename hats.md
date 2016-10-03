---
layout: default
title: Procrastinomics
subtitle: What I do When I Should Be Working
images:
 - "/img/sillyhats/IMG_2831.JPG" : "Russian"
 - "/img/sillyhats/small_IMG_0011.jpg" : "Viking"
 - "/img/sillyhats/small_IMG_0181_2.jpg" : "Sun"
 - "/img/sillyhats/small_IMG_0182_2.jpg" : "Monster"
 - "/img/sillyhats/small_IMG_0183_2.jpg" : "Leopard"
 - "/img/sillyhats/small_IMG_0184_2.jpg" : "Cowboy"
 - "/img/sillyhats/small_IMG_0185_2.jpg" : "Parrot"
 - "/img/sillyhats/small_IMG_0187_2.jpg" : "Captain Seuss"
 - "/img/sillyhats/small_IMG_0188_2.jpg" : "Tiny Sombrero"
 - "/img/sillyhats/small_IMG_0189_2.jpg" : "Chicken"
 - "/img/sillyhats/small_IMG_0190_2.jpg" : "Umbrella"
 - "/img/sillyhats/small_IMG_0988.jpg" : "Octopus"
 --
 
 <ul class="photo-gallery">
  {% for image in page.images %}
    <li><img src="{{ image.image_path }}" alt="{{ image.title}}"/></li>
  {% endfor %}
</ul>
