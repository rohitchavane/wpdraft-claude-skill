# WordPress Draft Publisher

Publish blog posts to WordPress as drafts. Supports document files (PDF/DOCX), Google Docs URLs, and text content.

## When the user provides a Google Docs URL:

1. Confirm it's a publicly shared Google Docs link (shared as "Anyone with the link can view")
2. Run the Google Docs processing command:

```bash
python3 -c "
from wordpress_publisher import process_and_publish_google_doc

result = process_and_publish_google_doc('GOOGLE_DOCS_URL_HERE')
"
```

This will automatically:
- Download the document via Google's export API (no login required)
- Preserve exact heading hierarchy (H1–H6), paragraphs, bold, italic, underline
- Extract all hyperlinks with anchor text
- Download and upload all images to WordPress Media Library (in their original positions)
- Convert embedded YouTube/Vimeo video links to WordPress-playable embeds
- Convert tables to HTML tables
- Create the draft post in WordPress

3. Share the edit URL with the user

## When the user provides a document file (PDF or DOCX):

1. First, confirm the file path with the user
2. Run the full document processing command:

```bash
python3 -c "
from wordpress_publisher import process_and_publish_document

result = process_and_publish_document('FILE_PATH_HERE')
"
```

This will automatically:
- Extract all text with proper formatting (headings, paragraphs)
- Extract and upload all images to WordPress Media Library
- Extract all hyperlinks with anchor text
- Convert tables to HTML tables
- Create the draft post in WordPress

3. Share the edit URL with the user

## When publishing a text draft (no document file):

Use this when I (Claude) wrote the blog post content:

```bash
python3 -c "
from wordpress_publisher import publish_blog_post

title = '''YOUR_TITLE_HERE'''

content = '''YOUR_HTML_CONTENT_HERE'''

excerpt = '''YOUR_EXCERPT_HERE'''

publish_blog_post(title, content, excerpt)
"
```

## What gets preserved from each source:

| Element | Google Docs URL | PDF | DOCX |
|---------|----------------|-----|------|
| Headings (H1-H6) | ✅ | ✅ | ✅ |
| Paragraphs | ✅ | ✅ | ✅ |
| Bold/Italic text | ✅ | ⚠️ Partial | ✅ |
| Hyperlinks | ✅ | ✅ | ✅ |
| Tables | ✅ | ⚠️ Partial | ✅ |
| Images (in position) | ✅ | ✅ | ✅ |
| Embedded videos | ✅ (as WP embeds) | ❌ | ❌ |

## Important Notes

- All posts are saved as **drafts** - never auto-published
- Images are uploaded to WordPress Media Library and embedded in their original positions
- For Google Docs: the doc must be shared as "Anyone with the link can view"
- User should review formatting in WordPress before publishing
- DOCX and Google Docs preserve more formatting than PDFs
