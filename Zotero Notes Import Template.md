---
Tags: zotero
DOI: {{DOI}}
Title: "{{title}}"
Authors: {{authors}}{{presenters}}
Year: {{date | format ("YYYY")}}
Type: {{itemType}}
Journal: {{publicationTitle}}
---

# {{title}}
{{pdfZoteroLink}}

> [!Cite] 
> {{bibliography}}

## Notes
---
{% persist "notes" %}{% endpersist %}


## PDF Annotations
---

{% for annotation in annotations-%} 
{%- if annotation.comment -%} 
{%- if annotation.annotatedText -%}
<span style="background-color:{{annotation.colorCategory}}">…</span> **{{annotation.colorCategory}} {{annotation.type}} | Page [{{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})** 

> {{annotation.annotatedText | escape }}

**Note:** {{annotation.comment}} 

---

{% endif -%}
{%- if annotation.imageRelativePath -%} 
📊 **Image Annotation**
> ![[{{annotation.imageRelativePath}}]]

**Note:** {{annotation.comment}}

---

{% endif -%} 
{%- endif -%}
{%- endfor -%}