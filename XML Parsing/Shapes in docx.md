Shapes in `.docx` files are handled using the DrawingML markup, a part of the Office Open XML standard. Shapes include things like rectangles, circles, arrows, and other vector-based graphics. These are typically stored in the `word/document.xml` file, referencing additional elements in the `word/_rels/` and `word/media/` directories.

Here's how shapes are represented in `.docx` XML:

---

### 1. **Basic Shape Structure**

Shapes are represented as `<w:drawing>` elements in the document. Inside `<w:drawing>`, the shape is defined using the **DrawingML namespace**.

#### Example: A Simple Rectangle Shape

```xml
<w:p>
  <w:r>
    <w:drawing>
      <wp:inline>
        <a:graphic xmlns:a="http://schemas.openxmlformats.org/drawingml/2006/main">
          <a:graphicData uri="http://schemas.openxmlformats.org/drawingml/2006/picture">
            <pic:pic xmlns:pic="http://schemas.openxmlformats.org/drawingml/2006/picture">
              <pic:nvPicPr>
                <pic:cNvPr id="1" name="Rectangle 1" />
                <pic:cNvPicPr />
              </pic:nvPicPr>
              <pic:blipFill>
                <a:blip r:embed="rId1" />
                <a:stretch>
                  <a:fillRect />
                </a:stretch>
              </pic:blipFill>
              <pic:spPr>
                <a:xfrm>
                  <a:off x="0" y="0" />
                  <a:ext cx="5486400" cy="3200400" />
                </a:xfrm>
                <a:prstGeom prst="rect">
                  <a:avLst />
                </a:prstGeom>
              </pic:spPr>
            </pic:pic>
          </a:graphicData>
        </a:graphic>
      </wp:inline>
    </w:drawing>
  </w:r>
</w:p>
```

---

### 2. **Key Elements in Shape XML**

#### a. `<wp:inline>`

Defines an inline drawing. It can also be `<wp:anchor>` if the shape is positioned freely relative to the page.

#### b. `<a:graphic>`

The container for the graphic content.

#### c. `<a:graphicData>`

Specifies the type of graphic (e.g., picture, chart, shape).

#### d. `<pic:pic>`

The picture or shape details:

- **`<pic:nvPicPr>`**: Non-visual properties (e.g., name, ID).
- **`<pic:blipFill>`**: Background fill for the shape. If an image is used, it's embedded and referenced by `r:embed`.
- **`<pic:spPr>`**: Shape properties, including geometry, transformations, and fills.

#### e. `<a:prstGeom>`

Specifies the shape geometry using the `prst` attribute:

- Common values: `rect`, `ellipse`, `roundRect`, `triangle`, etc.

---

### 3. **Positioning and Size**

#### a. Size

- **`<a:xfrm>`**: Specifies position (`<a:off>`) and size (`<a:ext>`).
    - `x` and `y`: Offset from the top-left corner (in EMUs).
    - `cx` and `cy`: Width and height (in EMUs).

1 EMU = 1/914400 inch.

#### Example

```xml
<a:xfrm>
  <a:off x="914400" y="914400" /> <!-- Position: 1 inch from the top and left -->
  <a:ext cx="1828800" cy="914400" /> <!-- Size: 2 inches wide, 1 inch high -->
</a:xfrm>
```

#### b. Anchoring

Shapes can either be **inline** or **floating**:

- **Inline**: Positioned within the text flow.
- **Floating**: Positioned relative to the page, margin, or another object using `<wp:anchor>`.

---

### 4. **Adding Colors and Fills**

#### a. Solid Fill

```xml
<a:solidFill>
  <a:srgbClr val="FF0000" /> <!-- Red color -->
</a:solidFill>
```

#### b. Gradient Fill

```xml
<a:gradFill>
  <a:gsLst>
    <a:gs pos="0">
      <a:srgbClr val="FF0000" /> <!-- Start color -->
    </a:gs>
    <a:gs pos="100000">
      <a:srgbClr val="0000FF" /> <!-- End color -->
    </a:gs>
  </a:gsLst>
</a:gradFill>
```

#### c. Image Fill

```xml
<pic:blipFill>
  <a:blip r:embed="rId1" />
  <a:stretch>
    <a:fillRect />
  </a:stretch>
</pic:blipFill>
```

---

### 5. **Relationship with `rId`**

Shapes often reference resources (e.g., images) via `rId` attributes. These are defined in the `word/_rels/document.xml.rels` file.

#### Example of a Relationship

```xml
<Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="media/image1.png" />
```

---

### 6. **Practice Examples**

#### a. Add a Rectangle to a Document

1. Create a shape using `<w:drawing>` and `<pic:pic>`.
2. Save the shape XML in `document.xml`.
3. Add relationships for any referenced media (e.g., images).

#### b. Modify an Existing Shape

Extract and edit the `<w:drawing>` element in `document.xml`.

---

### 7. **Tools for Debugging**

1. **Unzip `.docx` Files**: Use tools like `unzip` or 7-Zip to inspect XML files.
2. **Office Applications**: Create a document with shapes in Microsoft Word, then extract and study the resulting XML.
3. **Online Validators**: Use XML validators to check your XML syntax.

---

Would you like help writing a Rust function to handle shapes, or do you need more examples?