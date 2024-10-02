# Obsidian Zotero Integration Templates

The plugin [Obsidian Zotero Integration](https://github.com/mgmeyers/obsidian-zotero-integration) allows for seamless extraction of notes from PDF files in Zotero into Obsidian. This can be customized by setting up templates with various functions. However, not everyone is familiar with coding or template customization, which can make this process challenging. To simplify it, I’m sharing my custom script to assist colleagues in getting started easily.

## Zotero Notes Import Template

**Purpose:**  
To import PDF highlights and notes from Zotero into Obsidian with minimal clutter.

### Requirements:
- Zotero (installed and configured)
- Obsidian (installed and set up)
- [Obsidian Zotero Integration Plugin](https://github.com/mgmeyers/obsidian-zotero-integration)

### Features:
- **Custom YAML Metadata**: Automatically updates YAML with relevant information based on the item type.
- **Selective Import**: Only imports highlights that have notes attached, preventing unnecessary clutter on the page.
- **Image Support**: Allows importing images from PDFs (for example, drawing a rectangle around a figure in Adobe Acrobat).

## Detailed Breakdown

`Zotero Notes Import Template.md` serves as a **template** that dynamically generates both **YAML front matter** and corresponding **Markdown content** based on the metadata of Zotero items. This setup is particularly useful for creating well-structured and informative documents from bibliographic data, enabling seamless integration of references, abstracts, notes, and annotations into your documentation or static website.

### Table of Contents

- [1. YAML Front Matter](#1-yaml-front-matter)
    - [a. Aliases](#a-aliases)
    - [b. Tags](#b-tags)
    - [c. Extra Information for Specific Item Types](#c-extra-information-for-specific-item-types)
    - [d. Citation Key and Type](#d-citation-key-and-type)
    - [e. Authors and Participants](#e-authors-and-participants)
    - [f. Contributors and Recipients (Emails)](#f-contributors-and-recipients-specific-to-emails)
    - [g. Date Formatting](#g-date-formatting)
    - [h. Title and Subject](#h-title-and-subject)
    - [i. Publication Information](#i-publication-information)
    - [j. DOI and URL](#j-doi-and-url)
    - [k. Zotero Location and File Link](#k-zotero-location-and-file-link)
    - [l. Related Items](#l-related-items)
- [2. Markdown Content](#2-markdown-content)
    - [a. Conditional Sections Based on Item Type](#a-conditional-sections-based-on-item-type)
    - [b. Notes Section](#b-notes-section)
    - [c. Attachment Annotations](#c-attachment-annotations)


### 1. YAML Front Matter

The YAML front matter is enclosed between `---` lines and contains metadata that can be utilized by static site generators or other processing tools to manage and display content effectively.

#### a. Aliases

```yaml
aliases: 
- {% if title %}{{title | replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif caseName%}{{caseName | replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif subject%}{{subject | replace(':', '') | replace('[', '') | replace(']', '')}}{% endif %}
```

- **Purpose**: Defines alternative names or identifiers (`aliases`) for the item to facilitate easier referencing and searching.
- **Logic**:
  - **Primary Source**: Uses the `title` if available, removing colons (`:`) and square brackets (`[` and `]`).
  - **Fallbacks**: If `title` is unavailable, it checks for `caseName` or `subject`, applying the same cleaning process.
- **Outcome**: Ensures that aliases are clean, free from problematic characters, and provide meaningful alternative identifiers.

#### b. Tags

```yaml
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
```

- **Purpose**: Assigns relevant tags to the item for categorization and filtering.
- **Components**:
  - **Base Tag**: Always includes `zotero` to indicate the source.
  - **Conditional Tags**: Adds specific tags based on the `itemType`:
    - `emails` for emails.
    - `instant_messages` for instant messages.
    - `audio` for audio recordings.
  - **User-Defined Tags**: Iterates through any additional tags associated with the item and includes them.
- **Outcome**: Provides a robust tagging system that reflects both the item's type and any user-specified categories.

#### c. Extra Information for Specific Item Types

```yaml
{%- if itemType == "email" %}
{{extra}}
{%- endif %}
{%- if itemType == "instantMessage" %}
{{extra}}
{%- endif %}
{%- if itemType == "audioRecording" %}
{{extra}}
{%- endif %}
```

- **Purpose**: Inserts additional metadata (`extra`) specific to certain item types like emails, instant messages, and audio recordings.
- **Usage**: The `extra` field can contain supplementary information relevant to these item types, such as headers in emails or context in messages.
- **Outcome**: Enhances the metadata with type-specific details, ensuring comprehensive information capture.

#### d. Citation Key and Type

```yaml
citekey: {{citationKey}}
type: {{itemType}}
```

- **Purpose**:
  - **citekey**: Serves as a unique identifier for the citation, facilitating easy referencing in bibliographies or cross-references.
  - **type**: Specifies the Zotero item type (e.g., `email`, `journalArticle`, `audioRecording`), which can influence how the item is processed and displayed.
- **Outcome**: Provides essential identification and categorization for each item.

#### e. Authors and Participants

##### For Instant Messages

```yaml
{%- if itemType == "instantMessage" %}
participants:
{%- if creators.length > 0 %}
{%- for creator in creators %} 
{%- if creator.creatorType == "author" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %} 
{%- endfor %}
{%- endif %}
```

- **Purpose**: Lists all participants involved in an instant message.
- **Logic**:
  - Iterates through `creators` and includes those with `creatorType` as `author`.
  - Formats each participant's name within double square brackets (`[[ ]]`) for internal linking.
- **Outcome**: Provides a clear and linked list of participants for instant messages.

##### For Other Item Types

```yaml
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
{%- if creator.creatorType == "castMember" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- if creator.creatorType == "producer" %} 
- {% if creator.name %}"[[{{creator.name}}]]"{% else %}"[[{{creator.firstName}} {{creator.lastName}}]]"{% endif %}
{%- endif %}
{%- endfor %}
{%- endif %}
```

- **Purpose**: Compiles a comprehensive list of contributors based on their roles.
- **Roles Handled**:
  - `author`
  - `editor`
  - `director`
  - `castMember`
  - `producer`
- **Logic**:
  - Iterates through `creators` and includes those matching the specified `creatorType`.
  - Formats each name within double square brackets for linking.
- **Outcome**: Generates a detailed and linked list of contributors for various item types.

#### f. Contributors and Recipients Specific to Emails

```yaml
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
```

- **Purpose**: Specifically handles contributors and recipients in email items.
- **Components**:
  - **contributor**: Lists all contributors (`creatorType` == `contributor`) associated with the email.
  - **recipient**: Lists all recipients (`creatorType` == `recipient`) of the email.
- **Logic**:
  - Iterates through `creators` and segregates them based on `creatorType`.
  - Formats names within double square brackets for linking.
- **Outcome**: Provides a clear distinction between contributors and recipients in email items, enhancing the metadata's clarity and utility.

#### g. Date Formatting

```yaml
date: {{date | format ("YYYY-MM-DD")}}
```

- **Purpose**: Standardizes the `date` field to the `YYYY-MM-DD` format.
- **Usage**: Ensures consistency across all items, facilitating sorting and filtering based on dates.
- **Outcome**: Maintains uniform date formatting for better data management and readability.

#### h. Title and Subject

```yaml
{%- if itemType == "email" %}
subject: {{subject | replace(':', '') | replace('[', '') | replace(']', '')}}
{%- else %}
title: {% if title %}{{title| replace(':', '') | replace('[', '') | replace(']', '')}}{% elseif caseName %}{{caseName| replace(':', '')}}
{%- endif %}
{%- endif %}
```

- **Purpose**:
  - **Emails**: Uses the `subject` field.
  - **Other Items**: Uses the `title` or `caseName`.
- **Logic**:
  - Removes colons and square brackets from `subject`, `title`, or `caseName` to ensure clean metadata.
- **Outcome**: Provides a clear and clean title or subject for each item, enhancing readability and searchability.

#### i. Publication Information

```yaml
{%- if publicationTitle %}
publication: "[[{{publicationTitle| replace(':', '')}}]]"
{%- elseif seriesTitle %}
publication: "[[{{seriesTitle| replace(':', '')}}]]"
{%- elseif bookTitle %}
publication: "[[{{bookTitle| replace(':', '')}}]]"
{%- elseif forumTitle %}
publication: "[[{{forumTitle| replace(':', '')}}]]"
{%- endif %}
```

- **Purpose**: Specifies the publication source of the item.
- **Logic**:
  - Checks for various publication-related fields in a prioritized order:
    1. `publicationTitle`
    2. `seriesTitle`
    3. `bookTitle`
    4. `forumTitle`
  - Removes colons from the selected title.
  - Encloses the publication title within double square brackets for internal linking.
- **Outcome**: Accurately references the publication source, facilitating easy navigation and cross-referencing.

#### j. DOI and URL

```yaml
{%- if itemType == "journalArticle" %}
doi: {{DOI}}
{%- endif %}
{%- if itemType != "email" %}
url: {{url}}
{%- endif %}
```

- **Purpose**:
  - **DOI**: Included only for journal articles to provide a persistent link to the digital object.
  - **URL**: Included for all item types except emails to link to the resource.
- **Outcome**: Enhances accessibility by providing direct links to the item's digital location.

#### k. Zotero Location and File Link

```yaml
zotero_location: {{desktopURI}}
file_link: {% if attachments[0] %}file:///{{attachments[0].path | replace(' ', '%20')}}{% endif %}
```

- **Purpose**:
  - **zotero_location**: Provides the Zotero desktop URI for direct access to the item within Zotero.
  - **file_link**: Generates a file URL for the first attachment, replacing spaces with `%20` to ensure URL validity.
- **Outcome**: Facilitates quick access to the item's location in Zotero and its attachments, enhancing workflow efficiency.

#### l. Related Items

```yaml
{%- if relations.length >0 %}
related:
{%- for relation in relations %}
{%- if relation.citekey %}
- "[[@{{relation.citekey}}]]"
{%- endif %}
{%- endfor %}
{%- endif %}
```

- **Purpose**: Links related Zotero items by their `citekey`.
- **Logic**:
  - Iterates through `relations` and includes those with a valid `citekey`.
  - Formats each related item's `citekey` within double square brackets and prefixes with `@` for referencing.
- **Outcome**: Establishes connections between related items, enhancing the network of references and facilitating comprehensive data navigation.

### 2. Markdown Content

Following the YAML front matter, the template includes **Markdown content** that conditionally displays references, abstracts, summaries, notes, and attachment annotations based on the item's type and associated data.

#### a. Conditional Sections Based on Item Type

```markdown
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
```

- **Purpose**: Displays structured reference and abstract information based on the item's type.
- **Components**:
  - **Reference Section**:
    - **Header**: `[!Cite] Reference` indicates a citation block.
    - **Content**: `{{bibliography}}` inserts the formatted bibliography entry.
  - **Abstract/Summary Section**:
    - **Header**: `[!Abstract]- Abstract` or `[!Abstract]- Summary` depending on the item type.
    - **Content**: `{{abstractNote}}` displays the abstract or summary, with line breaks formatted for block quotes.
- **Logic**:
  - For specific item types (`book`, `bookSection`, `journalArticle`, `case`), it displays both a reference and an abstract.
  - For all other item types, it only displays a summary.
- **Outcome**: Provides clear and consistent presentation of references and abstracts, enhancing the document's informational value.

#### b. Notes Section

```markdown
## Notes

{% persist "notes" %}{% endpersist %}
```

- **Purpose**: Creates a section for notes associated with the item.
- **Components**:
  - **Header**: `## Notes` denotes a second-level heading in Markdown.
  - **Content**: `{% persist "notes" %}{% endpersist %}` is a placeholder for persisting notes. The specific functionality depends on the static site generator or processing tool being used.
- **Outcome**: Allocates a designated area for adding and storing notes related to the Zotero item.

#### c. Attachment Annotations

```markdown
{% if annotations.length > 0 -%}
## Attachment Annotations
{%- endif %} 
{% for annotation in annotations -%}
{%- if annotation.comment -%}
{%- if annotation.annotatedText %}
<span style="background-color:{{annotation.colorCategory}};color:Black">¶</span> **{{annotation.colorCategory}} {{annotation.type | capitalize}}** - Page [{% if annotation.page %}{{annotation.page}}{% else %}1{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})

{%- if annotation.tags.length > 0 %}

{% for antag in annotation.tags %}#{{antag.tag}} {% endfor %}

{%- endif %}

> {{annotation.annotatedText | escape }} 

**Note:** {{annotation.comment}} 


---
{% endif -%}
{%- endif -%}

{% if annotation.imageRelativePath %} 
📊 **Image** - Page [{{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})

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
<span style="background-color:{{annotation.colorCategory}};color:Black">◩</span> **{{annotation.colorCategory}} {{annotation.type | capitalize}}** - Page [{% if annotation.page %}{{annotation.page}}{% else %}1{% endif %}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})

{%- if annotation.tags.length > 0 %}

{% for antag in annotation.tags %}#{{antag.tag}} {% endfor %}

{%- endif %}

{%- if annotation.comment %} 

**Note:** {{annotation.comment}} 
{%- endif %}

---
{% endif -%}
{%- endfor -%}
```

- **Purpose**: Displays annotations associated with attachments (e.g., PDFs) in a structured and visually distinct manner.
- **Components**:
  - **Header**: `## Attachment Annotations` appears only if there are annotations.
  - **Annotation Entries**: Each annotation is processed and displayed based on its content and type.
- **Logic**:
  - **Annotation with Comment and Annotated Text**:
    - **Icon**: A styled paragraph symbol (`¶`) with a background color corresponding to the annotation's category.
    - **Header**: Displays the annotation's category and type (capitalized).
    - **Page Link**: Links to the specific page and annotation within Zotero.
    - **Tags**: Displays any associated tags as hashtags.
    - **Content**: Shows the annotated text in a block quote.
    - **Note**: Provides additional comments.
  - **Annotation with Image**:
    - **Icon**: A chart emoji (`📊`) indicating an image.
    - **Header**: Similar to text annotations, with a link to the specific page and annotation.
    - **Tags**: Displays associated tags.
    - **Content**: Embeds the image using the relative path.
    - **Note**: Includes any comments.
  - **Annotation of Type "Note"**:
    - **Icon**: A styled square symbol (`◩`) with corresponding background color.
    - **Header**: Displays category and type.
    - **Page Link**: Links to the specific page and annotation.
    - **Tags**: Shows associated tags.
    - **Note**: Includes comments.
- **Formatting**:
  - **Block Quotes**: Uses `>` to format annotated text and embedded images.
  - **Horizontal Rules**: `---` separates individual annotations for clarity.
  - **Emojis and Styled Icons**: Enhance visual distinction between different types of annotations.
- **Outcome**: Presents annotations in an organized, visually appealing manner, making it easy to review and navigate through notes, comments, and images associated with the item's attachments.

---

### Example Output

```markdown
---
aliases: Example Title
tags: 
- zotero
- emails
- important
citekey: ABC123
type: email
authors: 
- "[[John Doe]]"
contributor:
- "[[Jane Smith]]"
recipient:
- "[[Alice Johnson]]"
date: 2024-04-15
subject: Meeting Agenda
publication: "[[Company Newsletter]]"
url: https://example.com
zotero_location: zotero://select/items/XYZ789
file_link: file:///path/to/attachment.pdf
related:
- "[[@DEF456]]"
---

> [!Cite] Reference
> John Doe. *Example Title*. Company Newsletter, 2024.

> [!Abstract]- Abstract
> This document outlines the agenda for the upcoming meeting, including key discussion points and objectives.

## Notes

Your notes will appear here.

## Attachment Annotations

<span style="background-color:Yellow; color:Black">¶</span> **Yellow Highlight Note** - Page [5](zotero://open-pdf/library/items/XYZ789?page=5&annotation=12345)

#important 

> This section discusses the main goals for the meeting.

**Note:** Ensure all participants are prepared with their reports.

---
```

#### Contributing

Contributions are welcome! Please feel free to submit issues or pull requests to enhance the template's functionality or adapt it to additional use cases.

