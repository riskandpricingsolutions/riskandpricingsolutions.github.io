---
---

<h1 style="text-align:center">Risk And Pricing Solutions </h1>


<p style="text-align:center">.NET Consulting for the Financial Services Industry</p>



<script type="text/javascript"
    src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<p ç>
$$\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2S^2 \frac{\partial^2 V}{\partial t^2} + rS\frac{\partial V}{\partial S} -rV=0$$ 
 </p>

<h2>Personal Notes</h2>

<div>
{% if site.data.samplelist.toc2[0] %}
  {% for item in site.data.samplelist.toc2 %}
    <b>{{ item.title }}</b>
      {% if item.subfolderitems[0] %}
        <ul>
          {% for entry in item.subfolderitems %}
              <li>
               {% if entry.url %}
               <a href="{{ entry.url }}">{{ entry.page }}</a>
               {% else %}
               {{ entry.page }}
               {% endif %}
                {% if entry.subsubfolderitems[0] %}
                  <ul>
                  {% for subentry in entry.subsubfolderitems %}
                      <li><a href="{{ subentry.url }}">{{ subentry.page }}</a></li>
                  {% endfor %}
                  </ul>
                {% endif %}
              </li>
          {% endfor %}
        </ul>
      {% endif %}
    {% endfor %}
{% endif %}
</div>

<!-- <h2>Online Articles</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
      {{ post.excerpt }}
    </li>
    <br>
  {% endfor %}
</ul> -->