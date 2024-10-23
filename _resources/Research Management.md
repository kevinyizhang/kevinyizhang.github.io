---
title: "Research Management Tools"
collection: resources
permalink: /resources/research_tools
# type: "research_tools"
# venue: "University 1, Department"
# date: 2014-01-01
# location: "City, Country"
---
Research workflow differs from person to person. It is important to find one system that works for you the best. I personally use Zotero for reference management, Notion for events and notes, and Obsidian for writing in Markdown.

### Note Taking and Task Management
- Notion
  - [Tutorial 1](https://www.youtube.com/watch?v=tAJWxtBSaFQ&list=PLH1JWxs9OIRUn5e76Xn1zWyfMgaROINML) [Tutorial 2](https://www.youtube.com/watch?v=OUSv1wiW5uA&list=PLH1JWxs9OIRUn5e76Xn1zWyfMgaROINML&index=6)
  - I found these two tutorials particularly helpful for me to quickly grasp how Notion works behind the scene and how I can customize it quickly for it to function well.
  - Students can get free premium with a .edu email.
- Trello
- Asana
- Monday.com

### Reference and Reading Notes Management
- Zotero
- EndNotes
- Mendeley
- Zotero-Notion Workflow for Note Taking: [Tutorial](https://medium.com/@anna-everett/a-technical-guide-to-setting-up-notero-zotero-notion-plugin-d467d675039b)
  - The Notero integration enables you to sync a folder of literature in Zotero with a database in Notion.
  - It's a quick setup and you can also import notes (i.e., underlines, highlights, comments) from zotero.
- Zotero-Obsidian Workflow for Note Taking: [Tutorial](https://medium.com/@alexandraphelan/an-updated-academic-workflow-zotero-obsidian-cffef080addd)
  - The setup can be a little tricky though if you are using Obsidian plugins for the first time.
  - After installation, if it shows warning "Pandoc is not installed or accessible on your PATH." You will need to install the Pandoc software on your computer using the link [here](https://pandoc.org/installing.html). The file ending in .msi should work on windows.  After installation, reboot your Obsidian and the plugin should be able to work!
  - For some reason, the template provided in the tutorial was not working properly on my end. I asked ChatGPT for a slight revision, and it worked. For people who are newbies and get into the same issue, I have included my template in the very end of this page FYI.

#### My Obsidian Template for Zotero Integration
```
---
category: literaturenote
tags: {% if allTags %}{{allTags}}{% endif %}
citekey: {{citekey}}
status: unread
dateread:
---

> [!Cite]
> {{bibliography}}

>[!Synth]
>**Contribution**:: 
>
>**Related**:: {% for relation in relations | selectattr("citekey") %} [[@{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}
>

>[!md]
{% for type, creators in creators | groupby("creatorType") -%}
{%- for creator in creators -%}
> **{{"First" if loop.first}}{{type | capitalize}}**::
{%- if creator.name %} {{creator.name}}  
{%- else %} {{creator.lastName}}, {{creator.firstName}}  
{%- endif %}  
{% endfor %}~ 
{%- endfor %}    
> **Title**:: {{title}}  
> **Year**:: {{date | format("YYYY")}}   
> **Citekey**:: {{citekey}} {%- if itemType %}  
> **itemType**:: {{itemType}}{%- endif %}{%- if itemType == "journalArticle" %}  
> **Journal**:: *{{publicationTitle}}* {%- endif %}{%- if volume %}  
> **Volume**:: {{volume}} {%- endif %}{%- if issue %}  
> **Issue**:: {{issue}} {%- endif %}{%- if itemType == "bookSection" %}  
> **Book**:: {{publicationTitle}} {%- endif %}{%- if publisher %}  
> **Publisher**:: {{publisher}} {%- endif %}{%- if place %}  
> **Location**:: {{place}} {%- endif %}{%- if pages %}   
> **Pages**:: {{pages}} {%- endif %}{%- if DOI %}  
> **DOI**:: {{DOI}} {%- endif %}{%- if ISBN %}  
> **ISBN**:: {{ISBN}} {%- endif %}    

> [!LINK] 
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}
>  [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}})  {%- endfor -%}.

> [!Abstract]
> {%- if abstractNote %}
> {{abstractNote}}
> {%- endif -%}.
> 
# Notes
> {%- if markdownNotes %}
>{{markdownNotes}}{%- endif -%}.



# Annotations
{%- macro calloutHeader(type, color) -%}  
  {%- if type == "highlight" -%}  
    <mark style="background-color: {{color}}">Quote</mark>  
  {%- endif -%}

  {%- if type == "text" -%}  
    Note  
  {%- endif -%}  
{%- endmacro -%}

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

### Imported: {{importDate | format("YYYY-MM-DD h:mm a")}}

{% for a in newAnnotations %}
  {{calloutHeader(a.type, a.color)}}
  > {{a.annotatedText}}

  {%- if a.imageRelativePath -%}
  ![[{{a.imageRelativePath}}]]  <!-- Image is displayed here, right after the quote or note -->
  {%- endif %}

{% endfor %}
{% endif %}
{% endpersist %}

```
