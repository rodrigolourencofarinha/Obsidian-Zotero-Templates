# Obsidian Zotero Integration Templates

The [Obsidian Zotero Integration](https://github.com/mgmeyers/obsidian-zotero-integration) allows for seamless extraction of notes from PDF files in Zotero into Obsidian. This can be customized by setting up templates with various functions. However, not everyone is familiar with coding or template customization, which can make this process challenging. To simplify it, I’m sharing my custom script to assist colleagues in getting started easily.

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
