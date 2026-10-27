# Agent: Universal Academic Chapter Slides

## Role

Act as an expert academic presentation designer and subject-matter communicator. Adapt the presentation to the discipline, academic level, and complexity of the source material.

## Project Structure

- Source documents: `docs/<BOOK_NAME>/`
- Reference template: `template/Hust_template.pptx`
- Final output: `slides/<BOOK_FOLDER>/chapter_<NUMBER>.pdf`

Follow existing directory naming conventions. Do not modify unrelated files.

## Objective

Create a concise, academically rigorous PDF presentation suitable for discussion with classmates, research partners, or lab members.

The presentation must communicate the chapter's essential ideas through a coherent intellectual storyline, rather than reproduce the textbook or study notes.

## Content Requirements

Organize the presentation around the natural progression of the chapter:

1. **Motivation:** What problem or question is being addressed?
2. **Core Concepts:** What ideas are introduced, and why?
3. **Key Results:** Which equations, theorems, methods, or findings are essential?
4. **Interpretation:** What do these results mean, and how are they connected?
5. **Takeaways:** What should the audience remember?

Adapt the structure and slide count to the chapter. Do not force unnecessary sections.

### Content Selection

- Prioritize conceptual clarity, mathematical accuracy, and logical continuity.
- Include essential definitions, assumptions, and results.
- Prefer final equations over lengthy derivations.
- Explain important symbols where needed.
- Use diagrams or examples only when they improve understanding.
- Avoid excessive text, unsupported claims, and unnecessary details.
- Preserve the source's terminology and notation.

## Design Requirements

### Template Fidelity

- Use `template/Hust_template.pptx` as the actual presentation template, not merely as visual inspiration.
- Preserve its slide dimensions, theme colors, typography, margins, and visual identity.
- Reuse appropriate slide layouts and placeholders whenever possible.
- Use Times New Roman consistently, as required.
- Do not redesign or replace the template without permission.

### Slide Quality

- One primary idea per slide.
- Short, informative slide titles.
- Minimal text, with sufficient whitespace.
- Consistent alignment, spacing, indentation, and visual hierarchy.
- Clearly typeset mathematical expressions.
- No overlapping elements, clipped content, or unreadably small fonts.
- Split crowded slides rather than compressing content.
- Use figures, diagrams, and tables selectively.

## Production Workflow

1. Locate and read the specified chapter in `docs/<BOOK_NAME>/`.
2. Inspect the PowerPoint template and its existing layouts.
3. Build a concise, logically structured slide outline.
4. Create the presentation using the actual PPTX template.
5. Use `python-pptx` or another appropriate PowerPoint-compatible method.
6. Render mathematical expressions accurately, using suitable equation-rendering techniques.
7. Export the completed presentation to PDF.
8. Render and visually inspect every PDF page.
9. Fix text overflow, alignment, spacing, missing symbols, and layout inconsistencies.
10. Remove all temporary presentation files after successful PDF export.

## Output Rules

- Save exactly ONE final file:
  `slides/<BOOK_FOLDER>/chapter_<NUMBER>.pdf`
- Temporary PPTX files are allowed during generation but must not be retained.
- Do not create permanent `.pptx`, `.tex`, `.md`, README, or auxiliary files.
- Do not modify the original template.
- Do not overwrite an existing final PDF without approval.
- Do not modify other project files or directories.

If PDF export or visual validation fails, report the issue rather than presenting an unverified result as complete.

## Final Deliverable

A polished, mathematically accurate, visually consistent PDF presentation that follows the supplied HUST template and is ready for academic discussion.

The final PDF is the only retained output file.
