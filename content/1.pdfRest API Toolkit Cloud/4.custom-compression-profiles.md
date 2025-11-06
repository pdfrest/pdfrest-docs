## PDF Compression Levels

When you use the compression API, select the level of compression you want to apply to your PDF file in the `compression_level` setting.

We provide three standard values for compressing your PDF document:

1. `high` Make the PDF file as small as possible. This may reduce the quality of the output. If you choose `high`, you may see a difference in the way the PDF document renders when you print it or open it in a viewing tool.
2. `medium` A balance between `high` and `low` compression.
3. `low` Preserve the quality of the PDF file at the expense of file size optimization.

### Compression Effects

The `compression_level` parameter applies different effects to different resources within a PDF. The following is a list of how `high`, `medium`, and `low` affect resources such as images, transparencies, and more:


::tabs{variant="card"}
  ::div{label="Images"}
  The final quality of images in the PDF file after compression.

- `high` Minimum size. Compress images aggressively to reduce the size of the PDF file.
- `medium` A balance between high and low.
- `low` Maximum size. Protect the final appearance of the images at the cost of a larger file size.
  ::
  ::div{label="Fonts"}
The fonts that are included, removed, and/or subsetted.

- `high` Remove all unneeded fonts. Subset, remove unused fonts, consolidate duplicate fonts, and unembed base 14 fonts.
- `medium` Same as High, but don’t subset fonts.
- `low` No font changes.
  ::
  ::div{label="Objects"}
Discard objects embedded in the PDF document to make it smaller.

- `high` Throw away everything, including embedded JavaScript code, bookmarks, thumbnails, and other objects.
- `medium` Only remove alternate images.
- `low` No object changes.
  ::
  ::div{label="User Data"}
Discard metadata and information in the PDF document to make it smaller.

- `high` Throw away metadata, comments, attachments, and other specialized content.
- `medium` No user data changes.
- `low` No user data changes.
  ::
  ::div{label="Cleanup"}
General file compression settings.

- `high` All file compression settings turned on.
- `medium` All file compression settings turned on.
- `low` Only compress document structure and optimize file.
  ::
  ::div{label="Color Conversion"}
Adjusts whether color conversion is enabled.

- `high` Color conversion enabled. Decalibrate, switch to standard profile srgb.
- `medium` Color conversion turned off.
- `low` Color conversion turned off.
  ::
  ::div{label="Transparency"}
To flatten transparencies in a PDF input document, add one of these `quality`settings to your JSON profile file. If you don't include the `quality` setting in your JSON profile, the pdfRest API will not flatten transparencies.

- `high` Line art and text 1200 DPI, gradients 300 DPI
- `medium` Line art and text 300 DPI, gradients 150 DPI
- `low` Line art and text 288 DPI, gradients 144 DPI
  ::
::

## Customize Your Profile

Your JSON profile file should include a list of settings that define exactly what kinds of changes you want to apply to your PDF document. You can make your custom profile file as long or as short as you need, depending on the types of changes you plan to apply to optimize your PDF document.

We offer a variety of options to use when optimizing a PDF document. The options you select will depend on how you want to change your output PDF documents, and on your goals.

Suppose you have a PDF document that is 18 MB and you want to make it smaller so that the file will be easier to distribute online. If you expect that your readers will be opening the file in a browser window, and it does not matter if the photographs and diagrams in the document appear with a lower resolution, you could make the document smaller by compressing the images included in the file.

On the other hand, if you are working with a large PDF document that your customers are likely to want to print, but you want to make it smaller so that it downloads and prints more quickly, you probably want to leave the graphics alone. They will need to appear as sharp as possible. But you do not need interactive content, like form fields, bookmarks, comments, or digital signatures. You can use this Custom JSON profile feature to remove items from the PDF document that will not appear on paper.

Or maybe you are building a PDF document that you intend for people to read on smart phones and other mobile devices. In this case you want to compress the document so that it opens as quickly as possible. So you would reduce the size of the images in this case as well, but given that the screens are a lot smaller than a laptop or desktop monitor, you can reduce the resolution of the images in the PDF document to be less than a PDF document that is intended for opening in a browser window.

All of the settings for compressing a document are optional, and are turned `off` by default. That means then that a setting is only applied if it is included in the JSON file. Flag settings must be enabled, or set to `on` if the flag is an `on`/`off` value. Settings that are turned `off` do not need to be defined in the JSON profile file. So if you wanted to you could create a custom JSON file with only a single setting, to compress images. Your JSON file might only hold five or six lines of text.

::alert{type="secondary" icon="lucide:info"}
Reminder: A setting is only applied if it is included in the JSON file. Often, setting a paramter to `off` is the same as excluding it from the profile entirely.
::

Only use lowercase characters for the keys and values you add to the JSON profile file.

### Example JSON Profile

```bash [Custom JSON Profile]icon=lucide:code line-numbers height=350
{
    "images": {
        "color": {
            "downsample": {
                "trigger-dpi": 225,
                "target-dpi": 150
            },
            "recompress": {
                "type": "zip-jpeg",
                "quality": "medium"
            }
        },
        "grayscale": {
            "downsample": {
                "trigger-dpi": 225,
                "target-dpi": 150
            },
            "recompress": {
                "type": "zip-jpeg",
                "quality": "medium"
            }
        },
        "monochrome": {
            "downsample": {
                "trigger-dpi": 450,
                "target-dpi": 300
            },
            "recompress": {
                "type": "jbig2",
                "quality": "lossy"
            }
        },
        "optimize-images-only-if-reduction-in-size": "on",
        "consolidate-duplicate-objects": "on",
        "down-convert-16-to-8-bpc-images": "on"
    },
    "fonts": {
        "subset-embedded-fonts": "on",
        "consolidate-duplicate-fonts": "on",
        "unembed-standard-14-fonts": "on",
        "resubset-subset-fonts": "on",
        "remove-unused-fonts": "on"
    },
    "objects": {
        "discard-javascript-actions": "off",
        "discard-alternate-images": "on",
        "discard-thumbnails": "on",
        "discard-document-tags": "on",
        "discard-bookmarks": "off",
        "discard-output-intent": "on"
    },
    "userdata": {
        "discard-comments-forms-multimedia": "off",
        "discard-xmp-metadata-padding": "on",
        "discard-document-information-and-metadata": "on",
        "discard-file-attachments": "off",
        "discard-private-data": "on",
        "discard-hidden-layer-content": "off"
    },
    "cleanup": {
        "compression": "compress-entire-file",
        "flate-encode-uncompressed-streams": "on",
        "convert-lzw-to-flate": "on",
        "optimize-page-content": "on",
        "optimize-for-fast-web-view": "off"
    },
    "general": {
        "write-output-even-if-increase-in-size": "off",
        "preserve-version": "off"
    },
    "color-conversion": {
        "enabled": "off",
        "color-convert-action": "convert",
        "convert-intent": "profile-intent",
        "convert-profile": "srgb"
    },
    "pdfa-conversion": {
        "enabled": "off",
        "type": "1b",
        "pdfa-target-color-space": "rgb",
        "rasterize-if-errors-encountered": "off"
    }
}
```

## Optimization Parameters

### Parameter Categories

The methods you can use to optimize a PDF document are sorted into the following categories:
<br><br>

::collapsible
#title
### General

#content
These options affect the PDF document as a whole, rather than individual features such as images, fonts, or bookmarks. Settings provided under General allow you to force compression features that generate an output PDF document that may be larger in size than the input document.
<br>
::field-group
  ::field{name="write-output-even-if-increase-in-size" type="on/off"}
`off` If the API finds that the output file will be larger than the original PDF input file when it is finished processing, the system will not generate an output file at all. Rather, it will write an error message saying that the file could not be optimized.
<br><br>
`on` The API will generate the output file even if the optimization process produces a document larger than the input file.
  ::
  ::field{name="preserve-version" type="on/off"}
`off` The API saves the output document in v1.7 of the PDF standard.
<br><br>
`on` The API preserves the original version of the PDF source document if possible. If you select an option for the API to use when optimizing your source PDF document that will conflict with the current version of the PDF document, the API will ignore your request to preserve the original version of the PDF source document.

For example, if you want to use the API to process a PDF source document of version 1.5, and request that the version be preserved and also that the document be converted to PDF/A-3u format, the output will be 1.7 since it's required for PDF/A-3u output.
  ::
::
::

::collapsible
#title
### Fonts

#content
PDF documents can travel with the fonts that they need to access to properly render text. Font files can be embedded within the PDF itself. That way, no matter what machine is used to open a PDF file, the PDF is always guaranteed to look the same, and the viewing tool does not need to look for substitute fonts installed on the local desktop or laptop.
<br><br>
These embedded font files make the PDF larger, maybe a lot larger, if the document needs to express characters from an Asian font set, like Mandarin or Japanese. You can enter settings in your JSON profile file to remove individual font characters or sets that you don’t need, thus reducing the size of the PDF file.
<br>

::field-group
  ::field{name="subset-embedded-fonts" type="on/off"}
Subsetting fonts removes unused characters from font files embedded in the PDF.

It is a best practice when working with PDF documents to embed all of the fonts used in that document into the document itself. That way, a viewing tool (like Adobe Acrobat) does not have to look for a font on the local system, or choose a substitute. But embedding font files in a PDF document can make the PDF quite large, especially if the PDF has embedded a font file for an Asian language, such as Mandarin, with tens of thousands of characters.
<br><br>
To avoid this, a subset of the characters in the font can be saved in the PDF document. The subset font only includes the characters you expect to need when rendering the pages of that document. This often leads to a much smaller PDF.
  ::
  ::field{name="consolidate-duplicate-fonts" type="on/off"}
Remove multiple copies of the same font file.

Sometimes PDF documents are created with multiple copies of the same font, either as multiple subsets or multiple, fully embedded copies of a font file. When multiple copies of the same font appear, they may be merged into a single font.
  ::
  ::field{name="unembed-standard-14-fonts" type="on/off"}
Remove the Base 14 fonts. The Base 14 fonts are:
<br><br>`Times Roman`, `Helvetica`, `Courier`, `Symbol`, `Times Roman bold`, `Helvetica bold`,  `Courier bold`, `ZapfDingbats`, `Times Roman italic`, `Helvetica oblique`, `Courier oblique`, `Times Roman bold/italic`, `Helvetica bold/oblique`, `Courier bold/oblique`
<br><br>
When these standard fonts are embedded in a PDF document they can make the file larger.
  ::
  ::field{name="resubset-subset-fonts" type="on/off"}
Allows already subset fonts to be re-subset if possible (see `subset-embedded-fonts` above). Subsetting can significantly reduce the size of the PDF document if a font features thousands of glyphs, such as Mandarin. By re-subsetting a subset font in a PDF document, you are replacing it with a subset that will only contain the glyphs currently in use in the document.
<br><br>
Suppose you have a long PDF document that uses Mandarin characters. You decide to create a summary version of this file by deleting all but the first two pages. After re-subsetting, the Mandarin characters that no longer appear in the document are removed.
  ::
  ::field{name="remove-unused-fonts" type="on/off"}
Removes fonts that are embedded but not being used in the document.
  ::
::
::

::collapsible
#title
### Transparency

#content
It is possible to stack objects, such as graphics, images, text boxes, and form fields, on top of each other on a PDF document. These objects can be partially or fully transparent, and thus can interact in various ways with objects behind them. If a set of transparencies are stacked in a PDF file, each one contributes to the final result that appears on the page, such as the colors blending together into a final color that appears.
<br><br>
To simplify a PDF you can flatten these transparencies. The flattening process combines the layers of content on a PDF page, or a stack of transparent images or colors, and renders the result as a single image, blended color, or set of text. For example, if a digital signature is flattened, the digital certificate key and related properties are removed from the signature field. The name of the person who signed the document and related information, such as the date and time stamp and the signer’s email address, appear on the page as text, but the signature field is no longer interactive.

::field-group
  ::field{name="quality" type="string"}
The resolution level to use when flattening transparent objects. The higher the level of quality, the better the final output in print or in a browser window and the larger the resulting PDF will be.
<br>
- `high-quality` Line art and text 1200 DPI, gradients 300 DPI
- `medium-quality` Line art and text 300 DPI, gradients 150 DPI
- `low-quality` Line art and text 288 DPI, gradients 144 DPI
  ::
::
::

::collapsible
#title
### Objects

#content
Besides graphic images and font files, a variety of other objects can be saved within a PDF document. Examples include blocks of JavaScript code, thumbnail images, bookmarks, tags, and alternate graphics images.
<br><br>
Use the compression feature to remove any of these objects from a PDF document. This serves to make the document smaller and easier to distribute.

::field-group
  ::field{name="discard-javascript-actions" type="on/off"}
Removes JavaScript content. Blocks of JavaScript code can be added to a PDF to complete a function or calculate a value, such as a user’s age when that person enters his or her birth date in a form field.
  ::
  ::field{name="discard-alternate-images" type="on/off"}
Removes alternate versions of the same image found in the PDF document. A PDF document can be set up to specify alternate images, or multiple versions of one image within the same document. These images can be used to meet different needs. For example, a PDF could present one image with a lower resolution for display on a monitor, and an alternate image with a higher resolution to use when the PDF document is sent to a printer. These images can make a PDF document very large. Today alternate images are rarely used.
  ::
  ::field{name="discard-thumbnails" type="on/off"}
Removes document thumbnails. Thumbnail images are used to preview pages in a PDF document and appear in a panel on the left side of the viewer window. A user could scroll through a series of thumbnails to find a page they are looking for.
  ::
  ::field{name="discard-document-tags" type="on/off"}
Removes document tags. Tags are sometimes added to PDF documents to provide structural information to describe items such as headers and other content on a page. Tagging is generally used with a PDF document to meet accessibility requirements. For example, tags in a PDF document might be placed so that text, headings, footnotes, and other content in the document can be interpreted by a screen reading software tool.
  ::
  ::field{name="discard-bookmarks" type="on/off"}
Removes document bookmarks. Bookmarks make navigation easier. They are typically presented as a Table of Contents, and are commonly attached to headings within the document. A user typically interacts with this Table of Contents to move directly to the part of the page where the bookmark is found. And bookmarks can be used apart from a table of contents to mark a place in a PDF document to navigate to.
  ::
  ::field{name="discard-output-intent" type="on/off"}
Removes the document's output intent. The output intent is an entry in the PDF document OutputIntents array. The output intent is used to describe how the destination device for the document, most likely a printer, reproduces the colors in the document. Specifically, the output intent describes the ICC Color space to use for rendering the document. The ICC profile file is stored in the PDF document itself. If the output intent is present, rendering will be to that profile. PDF documents that are compliant with an ISO standard, like Graphics Exchange (PDF/X), Archive (PDF/A), and Engineering (PDF/E), often have an output intent set.
<br><br>
As the ICC profile can be quite large, you might want to remove it to reduce the size of the PDF document, if you don’t have plans to print the document in the future. But if it is important to preserve color fidelity for your application, it should not be removed.
  ::
::
::

::collapsible
#title
### User Data

#content

It is possible to edit PDF documents using Adobe Acrobat and other viewing and editing tools. For example, when reviewing the content in a PDF document, a user might want to add a comment. It is also possible to attach external files to a PDF document so that the file is saved as a part of the PDF, or embed a hyperlink to a web page. Finally, a user could add metadata. The compression feature can remove any of this content. It can also remove form fields, such as text boxes, check boxes, and radio buttons.
<br>
::field-group
  ::field{name="discard-comments-forms-multimedia" type="on/off"}
Removes interactive elements from the PDF document. These can include annotations, such as pop-up notes, comments, and highlights, as well as interactive Acrobat form fields and embedded multimedia. Besides removing the interactive elements, this option also removes the visual content associated with these elements.
  ::
  ::field{name="discard-xmp-metadata-padding" type="on/off"}
XMP refers to the Extensible Metadata Platform, a standard created by Adobe Systems to guide the creation, processing, and exchange of metadata for a variety of digital resources. When XMP metadata is included in a PDF document, the application that creates the PDF leaves a “padding area” in the text stream, commonly 2 to 4 KB for each set of XMP metadata. This allows for the metadata to be edited in place, and expanded if needed, without disturbing the document as a whole. This option removes the XMP padding from the document.
  ::
  ::field{name="discard-document-information-and-metadata" type="on/off"}
This option removes document descriptions and metadata.
  ::
  ::field{name="discard-file-attachments" type="on/off"}
This option removes files attached to the document.
  ::
  ::field{name="discard-external-crossreferences" type="on/off"}
Removes references to external data. This would include links to external resources, like a photograph or another PDF document that could be downloaded from a web page or FTP site. This option effectively removes the hyperlinks to these items.
  ::
  ::field{name="discard-private-data" type="on/off"}
Removes piece data relevant to the application that created the file. Some applications, like Adobe Illustrator, add their own unique content to a PDF document when generating that document. This information is useful to the original software product if the PDF is opened and edited in that product again. But this information can be removed.
  ::
  ::field{name="discard-hidden-layer-content" type="on/off"}
Layers in a PDF document allow content to be placed above or below other content. The content in layers can be hidden from view, so that, for example, copyright information provided on the bottom of a page does not appear in Adobe Reader but is included if the document sent to a printer. The layers in a PDF document can be displayed in the Layers panel in Adobe Acrobat. You can remove this hidden content to reduce the size of a PDF document.
  ::
::
::

::collapsible
#title
### Cleanup

#content

Use the Cleanup features to set compression values for a PDF document. You can compress the entire PDF document or parts of the content, and you can also remove redundant content or select a compression method to use, as well as other changes designed to make a PDF document open more quickly.
<br>
::field-group
  ::field{name="compression" type="string" required}
Select the compression action for the file. Enter one of these values:
<br><br>
- `compress-entire-file` Compress document as a single unit
- `compress-document-structure` Compress the document structure only
- `remove-compression` Removes compression from file streams
  ::
  ::field{name="flate-encode-uncompressed-streams" type="on/off"}
Compress uncompressed streams using Flate.
<br><br>
Flate, or Deflate compression, is an open source standard widely used for creating zip files and in PDF documents. It is commonly used for PNG image files, and is much more widely used than LZW.
  ::
  ::field{name="convert-lzw-to-flate" type="on/off"}
Recompress LZW-compressed streams using Flate.
<br><br>
Flate, or Deflate compression, is an open source standard widely used for creating zip files and in PDF documents. It is commonly used for PNG image files, and is much more widely used than LZW.
<br><br>
LZW, Lempel-Ziv-Welch, is a universal data compression algorithm, once widely used with Unix platforms. This method appears in some old PDF documents but it is rarely used any longer.
  ::
  ::field{name="optimize-page-content" type="on/off"}
Removes redundant content streams, or page text.
  ::
  ::field{name="optimize-for-fast-web-view" type="on/off"}
Place all the information needed to render the first page of the document near the beginning of the file. This process is also known as **linearization**. This allows a web browser to open the PDF document more quickly.
  ::

::
::

::collapsible
#title
### Images

#content

When a PDF document is created that includes photographs, diagrams or drawings, the original graphic file, such as a JPEG photograph or a PNG image, become images in that PDF file. You can enter settings in the JSON profile file to compress these color, grayscale, or monochrome (black and white) images.
<br>
::alert{type="secondary" icon="lucide:info"}
See additional details about [Image Compression below](/additional-guides/compress-pdf-custom-profiles#image-compression-details).
::
<br>
Select one of `color`, `grayscale`, `monochrome` and then set `downsample` and/or `recompress`.

```bash [Images Sample JSON]icon=lucide:code line-numbers
# A code sample which shows `color` compression with both `downsample` and `recompress` turned on.

"images": {
    "color": {
        "downsample": {
            "trigger-dpi": 225,
            "target-dpi": 150
        },
        "recompress": {
            "type": "zip"
            "quality": "medium"
        }
    },
}
```

::field-group
  ::field{name="color downsample" type="intiger"}
Ability to specify a target resolution and a trigger resolution at which color images will be recompressed.
<br><br>
- `trigger-dpi` Number representing the Dots per Inch. All color images above this resolution will be downsampled.
- `target-dpi` Number representing the Dots per Inch. The new resolution of downsampled color images.

```bash [Color Downsample Example]icon=lucide:code line-numbers
"color": {
    "downsample": {
        "trigger-dpi": 225,
        "target-dpi": 150
},
```
  ::field{name="color recompress" type="string"}
Sets the type and quality of compression used to downsample color images. JPEG compression is a compression format commonly used for photographs. It uses a technique known as DCT, Discrete Cosine Transform.
<br><br>
- `type` One of the following: `same`, `zip`, `jpeg`, `jpeg2000`, `zip-jpeg`
- `quality` These values are valid for JPEG and JPEG2000 compression only. One of the following: `minimum`, `low`, `medium`, `high`, `maximum`, `lossless`
<br><br>

With `quality`: `lossless` the original quality of the graphic is preserved (see Monochrome). Only available for JPEG2000.

```bash [Color Recompress Example]icon=lucide:code line-numbers
"color": {
    "recompress": {
        "type": "jpeg",
        "quality": "medium"
},
```
  ::
  ::field{name="optimize-images-only-if-reduction-in-size" type="on/off"}
Downsample an image found in a PDF document if the newly downsampled image is in fact smaller than the original. When compressing an image, the output file can possibly expand in size. If the process yields an image that is the same size as the original, or larger, the API will leave the image alone.
  ::
  ::field{name="consolidate-duplicate-image-and-forms" type="on/off"}
Remove duplicate copies of alternate images and forms.
  ::
  ::field{name="down-convert-16-to-8-bpc-images" type="on/off"}
Convert images that are 16 bits per component to 8 bits per component.
<br><br>
The color depth of an image is the number of bits used per pixel for each color component. RGB, for example, has three color components. By down-converting an image in a PDF file from 16 bpc to 8 bpc, you are reducing the resolution of the image, but also significantly reducing its size. If a PDF document features high-resolution images, the final PDF can also be significantly smaller.
<br><br>
This feature is not applicable if Color Conversion is enabled (see Color Conversion below). Color Conversion will attempt to down-convert 16 bpc images automatically if you turn it on. The down-convert feature only has an impact if Color Conversion is turned off.
  ::

::
::

::collapsible
#title
### Color Conversion

#content

Convert the colors in a PDF document to a color profile you select *before* compressing that document.
<br><br>
We can see thousands of different shades of colors, and high-quality digital cameras and scanners can often detect millions of shades. To manage the broad range of colors for producing graphics images in digital content, imaging professionals have developed models to define these colors, called color spaces.
<br><br>
Many color spaces have been defined. Some are dependent on hardware devices, and define what a camera can detect, or a printer print, or a monitor display. Others are based on software and thus can be used across many different types of devices, such as Adobe RGB or sRGB (standard RGB). A color space must be defined for any device or software product to make sure that coloring patterns remain the same from one device or system to another.
<br><br>
The Standard RGB color space, sRGB, was developed by Microsoft and Hewlett Packard to describe colors available on most monitors and displays. This color space is also commonly used for web graphics.
<br><br>
Adobe Systems’ own Adobe RGB (Red/Green/Blue) color space is designed to hold all of the colors that are likely to be available on any color CMYK (Cyan/Magenta/Yellow/Black) printer. It is considerably larger than Standard RGB.
<br><br>
Color profiles are standards for managing colors, used to ensure that the colors for text or graphics in a file remain the same regardless of the hardware or software used to display, edit, or print that file. Color profiles are based on the specification created by the International Color Consortium (ICC) in 1993 to govern color and color management across all operating systems, platforms, and software and hardware and software systems.
<br><br>
A color profile is usually expressed as a file included in the software or driver for an installed printer, scanner or other hardware device, or in software used to edit a file that is to be displayed or printed. A profile provides a set of data that describes an input or output device. A color profile file can also be embedded in a PDF document.

```bash [Color Conversion Example]icon=lucide:code line-numbers
"color-conversion": {
    "enabled": "on",
    "color-convert-action": "convert",
    "convert-intent": "profile-intent",
    "convert-profile": "acrobat9-cmyk"
}
```

::field-group
  ::field{name="enabled" type="on/off"}
Convert colors in a PDF document using a target profile.
  ::
  ::field{name="convert-profile" type="string"}
The profile used to convert colors in the PDF document. Set one of:
<br>
- `lab-d50`
<br>Lab color specification with a D50 white point. The Lab color space is based on the CIE XYZ color space, but it includes a dimension L, for lightness, along with a and b coordinates, to define the color. This is Adobe Systems’ standard Lab profile.<br><br>
- `srgb`
<br>Standard RGB, the default profile for Windows monitors.<br><br>
- `apple-rgb`
<br>Apple RGB, the default profile for Mac monitors.<br><br>
- `color-match-rgb`
<br>Color Match RGB. This is a simpler version of the Radius ColorMatch RGB space, without the non-zero black point.<br><br>
- `monitor-rgb`
<br>RGB Monitor, referring to a monitor that requires separate signals for the three primary colors.<br><br>
- `gamma-18`
<br>Gray Gamma 1.8, grayscale display profile, used for content viewed on a monitor.<br><br>
- `gamma-22`
<br>Gray Gamma 2.2<br><br>
- `dot-gain-10/15/20/25/30`
<br>Grayscale printer profile, with dot gain X%. Dot gain is commonly used in offset printing to define the increase in size in halftone dots in the printing process, making a printed document look darker than intended. For example, `dot-gain-20` will gain 20%.<br><br>
- `acrobat5-cmyk`
<br>Adobe Reader 5 CMYK<br><br>
- `acrobat9-cmyk`
<br>Adobe Reader 9 CMYK
<br><br>

The above list of options represents the standard list of color profiles provided with the API. Select one of these profiles, specifying it by name in your JSON profile.

  ::
  ::field{name="color-convert-action" type="string"}
The type of color conversion. The values available include `convert` and `decalibrate`. The `decalibrate` setting will decalibrate calibrated color spaces in the PDF document.
<br><br>
If a color profile file is assigned to a PDF, or to a feature within that PDF document such as an image, the PDF is referred to as “calibrated.” This color profile will be used when rendering colors for that PDF.
<br><br>
If a color profile is not assigned to the document, a default color profile is used instead, usually installed on the local device or on a printer. So if you select `decalibrate` as the color-conversion-option, you can remove the ICC profile file stored in the PDF document, and thus reduce the size of the PDF output file.
  ::
  ::field{name="convert-intent" type="string"}
Use this setting to specify the color translation method for colors that are outside the gamut of the color profile. The intent lets the software determine how to substitute a color that can be written to the file. You can select from a list of standard strategies to apply when converting the colors in that original PDF document.
<br><br>
The conversion intent is used to describe how the destination device for the document reproduces the colors in the document. Thus, when you print or display the PDF output file, the colors in the output file will match as closely as possible the original color found in the source PDF document.
<br><br>
Set to one of:

- `perceptual-intent`
<br>Generally used for photography. This method does not map colors one for one but estimates to match colors. Hence it often provides the most pleasing result but not necessarily the most accurate.<br><br>
- `relative-colorimetric-intent`
<br>Generally used for photography. The relative method uses an algorithm to select the closest possible color map to be true to the specified color.<br><br>
- `saturation-intent`
<br>Commonly used in charts and diagrams with a limited palette of colors where hue is not as important.<br><br>
- `absolute-colorimetric-intent`
<br>Often used to select a specific color or set of colors for drawings or designs. Absolute serves to reproduce the exact colors provided in the original PDF document. A common reason for using absolute would be to reproduce the color used in a corporate logo such as IBM Blue. The color is changed by selecting a defined match. This method does not use a conversion algorithm to select the closest color available.<br><br>
- `profile-intent`
<br>If you specify a color profile in the convert-profile option the intent value defaults to that profile. In that case software will use the rendering intent provided with the ICC color profile currently in use for that PDF document. For example, the Adobe RGB 1998 color profile uses Relative Colormetric as its rendering intent. So if you specify Adobe RGB 1998 (srgb) as the color profile, and profile-intent is selected here, the API will use relative.
  ::

::
::

### Image Compression Details

#### Downsampling Images

If you have images in a PDF document that you want to make smaller, and you know that these images don’t need to have a high resolution in the output file, you can reduce the resolution of these images. You can also compress these images within the file. Both steps will reduce the final size of the PDF document.

The process of reducing the resolution of images is called downsampling. You can choose to downsample color images in a PDF document, or grayscale, or monochrome (black & white). The settings for reducing the resolution for these three kinds of images in a PDF document must be added separately to the JSON profile file. Each type of image can have its own settings and resolution values. So you could, for example, enable resampling to only apply to the color images in a PDF document. Or you could include only grayscale and black and white images.

#### Downsampling and Recompression

Downsampling reduces the size of the image directly by reducing the resolution. In recompression, compressed images in a document are decompressed and then compressed again. You can enter a recompression setting to change the compression algorithm used for recompression, such as ZIP, JPEG or Flate, and another setting to change the final image quality. The image quality is part of the compression method used.

If you add settings in the JSON profile file to downsample images, the Datalogics PDF REST APIs will also recompress the images involved whether you provide recompression settings or not.

If you do not add recompression settings to the JSON profile, the API downsamples and recompresses each image in the PDF document using the default compression algorithm and quality value defined in the image itself. For example, if you provide downsample settings but not recompression settings in your JSON profile, and apply that profile to a document that only holds images using JPEG compression, the API will use the JPEG compression method. It will also use the highest quality recompression setting available (“maximum”) to keep from reducing the quality of the images as they are recompressed.

On the other hand, if you decide to leave out downsample settings from your JSON profile file, but add recompression settings, the API will recompress the images using the recompression algorithm you provide while keeping the image downsampling resolution (DPI) the same. Note that if you add recompression settings you must include both values in the JSON file, the compression algorithm and the recompression quality level.

#### Image Resolution

When we refer to the resolution of an image, we generally refer to the number of pixels in that image. This can be expressed in terms of megapixels, or in Dots per Inch (DPI). With an image in a PDF document, the resolution of the image is expressed as a certain number of pixels wide and pixels high. The downsampling process involves changing the width and height of an image in pixels, in order to reach a given target resolution. The API calculates the resolution for every image in the document. Keep in mind that the resolution values used with downsampling are distinct from the image quality settings used for image recompression.

You can specify a target resolution to use for downsampling images in a document (target-dpi) and a trigger resolution (trigger-dpi). If you decide to downsample an image type, both the target and the trigger resolution settings must be included in your profile file. The target resolution defines the goal—the maximum resolution for every image in the file. So if you add a target resolution to your JSON profile and set that target resolution to 600 DPI, the API will downsample every graphic in the PDF document to 600 DPI unless it that image is already at 600 DPI or less.

The trigger resolution, if used, defines the resolution the API uses as its starting point. Any image with a resolution greater than the trigger resolution will be downsampled. If an image has a resolution less than the trigger resolution, PDF Optimizer ignores it.

So if you set the trigger resolution to 800 DPI, and the target resolution to 400 DPI, it means that you want to downsample every image in the PDF document to 400 DPI, but only if the image is larger than 800 DPI to begin with. In this example you would be telling the API to look for only the really large images (the ones with a resolution at 800 DPI or more) and then downsample just those images to a certain set value, in this example 400 DPI.

If the trigger resolution is 500 DPI, and the target resolution is 400 DPI, the API will not downsample an image if it is 480 DPI. But if the trigger resolution is 500 and the target is 400, if the API finds an image with a resolution of 680 DPI, it will downsample it to 400 DPI.