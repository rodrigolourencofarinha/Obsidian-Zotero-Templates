---
aliases: 
- {% if title %}{{title | replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif caseName%}{{caseName | replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif subject%}{{subject | replace(':', '') | replace('[', '') | replace(']', '')}}{% endif %}
- {% if bibliography %}{{bibliography | replace(':', '') | replace('[', '') | replace(']', '')| replace('_', '')}}{% endif %}
tags: 
- zotero
{%- if itemType == "email" %}
- emails
{%- endif %}
{%- if itemType == "instantMessage" %}
- instant_messages
{%- endif %}
{%- if itemType == "audioRecording" %}
- audio
{%- endif %}
{%- if tags.length > 0 %}
{%- for tag in tags %}
- {{tag.tag}}
{%- endfor %}
{%- endif %}
{%- if itemType == "email" %}
{{extra}}
{%- endif %}
{%- if itemType == "instantMessage" %}
{{extra}}
{%- endif %}
{%- if itemType == "audioRecording" %}
{{extra}}
{%- endif %}
citekey: {{citationKey}}
type: {{itemType}}
{%- if itemType == "interview" %}
{%- if creators.length > 0 %}
interviewee:
{%- for creator in creators %} 
{%- if creator.creatorType == "interviewee" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
interviewer:
{%- for creator in creators %} 
{%- if creator.creatorType == "interviewer" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
{%- endif %}
{%- else %}
{%- if itemType == "instantMessage" %}
participants:
{%- if creators.length > 0 %}
{%- for creator in creators %} 
{%- if creator.creatorType == "author" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
{%- endif %}
{%- else %}
authors: 
{%- if creators.length > 0 %}
{%- for creator in creators %} 
{%- if creator.creatorType == "author" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- if creator.creatorType == "editor" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "director" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "contributor" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "producer" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "castMember" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "producer" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- endfor %}
{%- endif %}
{%- if itemType == "email" %}
{%- if creators.length > 0 %}
contributor:
{%- for creator in creators %} 
{%- if creator.creatorType == "contributor" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
recipient:
{%- for creator in creators %} 
{%- if creator.creatorType == "recipient" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
{%- endif %}
{%- endif %}
{%- endif %}
{%- endif %}
date: {{date | format ("YYYY-MM-DD")}}
{%- if itemType == "email" %}
subject: {{subject | replace(':', '') | replace('[', '') | replace(']', '')}}
{%- else %}
title: {% if title %}{{title| replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif caseName %}{{caseName| replace(':', '')}}
{%- endif %}
{%- endif %}
{%- if publicationTitle %}
publication: "[[{{publicationTitle| replace(':', '')}}]]"
{%- elseif seriesTitle %}
publication: "[[{{seriesTitle| replace(':', '')}}]]"
{%- elseif bookTitle %}
publication: "[[{{bookTitle| replace(':', '')}}]]"
{%- elseif forumTitle %}
publication: "[[{{forumTitle| replace(':', '')}}]]"
{%- endif %}
{%- if itemType == "journalArticle" %}
doi: {{DOI}}
{%- endif %}
{%- if itemType != "email" %}
url: {{url}}
{%- endif %}
location: {{desktopURI}}
{%- if relations.length >0 %}
related:
{%- for relation in relations %}
{%- if relation.citekey %}
- "[[@{{relation.citekey}}]]"
{%- endif %}
{%- endfor %}
{%- endif %}
---
{{pdfURI}}
{%- if itemType == "book" %}

> [!Cite] Reference
> {{bibliography}}

> [!Abstract]- Abstract
> {{abstractNote | replace("\n", "\n> ")}}

{%- elseif itemType == "bookSection" %}

> [!Cite] Reference
> {{bibliography}}

> [!Abstract]- Abstract
> {{abstractNote | replace("\n", "\n> ")}}

{%- elseif itemType == "journalArticle" %}

> [!Cite] Reference
> {{bibliography}}

> [!Abstract]- Abstract
> {{abstractNote | replace("\n", "\n> ")}}

{%- elseif itemType == "case" %}

> [!Cite] Reference
> {{bibliography}}

> [!Abstract]- Abstract
> {{abstractNote | replace("\n", "\n> ")}}

{%- else %}

> [!Abstract]- Summary
> {{abstractNote | replace("\n", "\n> ")}}

{%- endif %}
## Notes

{% persist "notes" %}{% endpersist %}

## AI Summary

{% persist "summary" %}{% endpersist %}

{% if annotations.length > 0 -%}

## Attachment Annotations
{%- endif %} 
{% for annotation in annotations -%}
{%- if annotation.comment -%}
{%- if annotation.annotatedText %}
<span style="background-color:{{annotation.colorCategory}};color:Black">¶</span> **{{annotation.colorCategory}} {{annotation.type | capitalize}}** - {% if pdfLink %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Page 1{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% else %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Position{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% endif %}

{%- if annotation.tags.length > 0 %}

{% for antag in annotation.tags %}#{{antag.tag}} {% endfor %}

{%- endif %}

> {{annotation.annotatedText | escape }} 

**Note:** {{annotation.comment}} 


---
{% endif -%}
{%- endif -%}

{% if annotation.imageRelativePath %} 
📊 **Image** - {% if pdfLink %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Page 1{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% else %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Position{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% endif %}

{%- if annotation.tags.length > 0 %}

{% for antag in annotation.tags %}#{{antag.tag}} {% endfor %}

{%- endif %}

> ![[{{annotation.imageRelativePath}}]]

{%- if annotation.comment %} 

**Note:** {{annotation.comment}} 
{%- endif %}

---
{% endif -%}

{% if annotation.type == "note" %} 
<span style="background-color:{{annotation.colorCategory}};color:Black">◩</span> **{{annotation.colorCategory}} {{annotation.type | capitalize}}** - {% if pdfLink %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Page 1{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% else %}[{% if annotation.page %}Page {{annotation.page}}{%- else %}Position{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){% endif %}

{%- if annotation.tags.length > 0 %}

{% for antag in annotation.tags %}#{{antag.tag}} {% endfor %}

{%- endif %}

{%- if annotation.comment %} 

**Note:** {{annotation.comment}} 
{%- endif %}

---
{% endif -%}
{%- endfor -%}
