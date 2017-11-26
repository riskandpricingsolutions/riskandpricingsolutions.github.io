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



<ul>
  {% for post in site.posts %}
    <li>
      <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
      {{ post.excerpt }}
    </li>
    <br>
  {% endfor %}
</ul>