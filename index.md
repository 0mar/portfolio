---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: splash
author_profile: true
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/header.png
excerpt: "Omar Richardson's personal blog on research and development in AI, applied mathematics and digital innovation"
intro: 
  - excerpt: 'As part of my work in the Department of Applied AI at Simula, I work on R&D projects in various domains, usually involving machine learning and app development. This blog has details on those projects, as well as stand-alone blog posts on software development and applied mathematics'
feature_row:
  - image_path: assets/fish.png
    alt: "computer vision for biodiversity"
    title: "Combatting invasive species with machine learning"
    excerpt: "Protecting biodiversity using computer vision and underwater robotics"
    url: /projects/albatross
  - image_path: /assets/losjitech.png
    alt: "AI agent frameworks"
    title: "LLM agents to generate structured information"
    excerpt: "Building conversational pipelines to interface between end-user and APIs with large language models"
    url: /projects/smart-assistent
  - image_path: /assets/jupyterhealth.png
    title: "Democratizing health care data with JupyterHealth"
    excerpt: "Share wearable health data securely between patient, healthcare provider, and researcher"
    url: /projects/jupyterhealth
---
{% include feature_row id="intro" type="center" %}

# Recent projects
{% include feature_row %}

# Recent posts
<div class="feature__wrapper">
  {% for post in site.posts limit:3 %}
      {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>
