---
---

Attempt: 23

{% comment %}
Try to eat up all leading slashes and the trailing .html . 
This is gross but it works.
{% endcomment %}
{% assign ward-id = page.url | replace: '.html', '' | split: '/' | last -%}
{% assign ward-info = site.data.site-data.position-tags |
where:"PositionUniqueName",ward-id | first -%}

## Running in this Ward

{% assign races = site.data.site-data.municipality-map |
where:ward-info.WardMunicipality,"Name" %}
{% assign race-array = races | split: ',' %}

Races: {{ races }} 
Race Array: {{ race-array }}

{% for race in race-array -%}
  {% if race == "_SELF" %}
    {% assign race = ward-id %}
  {% endif %}
  
  {% assign race-info = site.data.site-data.position-tags |
  where:"PositionUniqueName",race -%}

  ### {{ race-info.PositionDesc -}}
  

  {% assign these-nominees = site.data.site-data.nominees 
    | where:"PositionUniqueName",race %}
  {% assign sorted-nominees = these-nominees | sort: "Last_Name" %}

  {% for nominee in sorted-nominees -%}
  - {% if nominee.Website -%}
    [{{ nominee.Given_Names }} 
      {{ nominee.Last_Name }}]({{ nominee.Website }})
    {%- else -%}
      {{ nominee.Given_Names}} {{ nominee.Last_Name }}
    {%- endif -%}
  {% endfor %}

{% endfor %}


## All positions

{% for position in site.data.site-data.position-tags %}
- {{ position.PositionUniqueName }} : {{ position.PositionDesc }}
{% endfor %}

## All nominees 

{% for nominee in site.data.site-data.nominees %}
- {{ nominee.Given_Names }} {{ nominee.Last_Name }} : 
  {{- nominee.PositionUniqueName }}
{% endfor %}