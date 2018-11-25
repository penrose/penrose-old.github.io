---
title: Welcome
permalink: /
---

Penrose is software that enables people to <b>create beautiful diagrams 
just by typing mathematical notation in plain text.</b> The goal is to make it 
easy for non-experts to create and explore high-quality diagrams, providing 
deeper insight into challenging technical concepts. We aim to <b>democratize 
the process of creating visual intuition</b>.

> Where do I go from here?

If you are new to penrose, we recommend you start with 
the [getting started]({{ site.baseurl }}/getting-started/) pages.

<b>News:</b> We've released a <a href="http://penrose.ink/PenroseIntro2018.pdf">short introduction</a> 
to the latest design of Penrose.

<b>Contribute:</b> Do you often make diagrams of ideas in math, science, or 
computer science for learning, research, or teaching? We'd like to 
<a href="http://penrose.ink/visualization-interview">interview you</a> as part 
of a study to inform the design of Penrose. Feel free to [Get In Touch](mailto:{{ site.author.email }}).


<b>About:</b> The Penrose team is based at Carnegie Mellon University, 
comprising {% for person in site.data.people.current %}
<a href="{{ person.url }}">{{ person.name }}</a>{% endfor %}
Emeritus: ({% for person in site.data.people.emeritus %}
<a href="{{ person.url }}">{{ person.name }}</a>{% endfor %})

Preliminary work on Penrose may be found in this two-page <a href="http://penrose.ink/Penrose_DSLDI.pdf">proposal</a> (in <i>DSLDI '17</i>) and the accompanying <a href="http://penrose.ink/Penrose_DSLDI_slides.pdf">slides</a>. For a very early overview, see the two-page <a href="https://www.cs.cmu.edu/~kqy/resources/Penrose_OBT.pdf">proposal</a> (in <i>OBT '17</i>) and the accompanying <a href="https://www.cs.cmu.edu/~kqy/resources/Penrose_PLunch_slides.pdf">slides</a>. <b>Penrose is an early-stage system that is still in development.</b> We're building it as fast as we can. If you follow the <a href="https://github.com/penrose/penrose">repository</a> and join the mailing list, you'll be the first to hear any news.

{% include mailchimp.html %}
