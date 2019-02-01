# Variable Font Collection Test

The purpose of this repository's OpenType/CFF2 Collections (aka Variable Font Collections) is to simulate the deployment format of our open source [*Source Han Sans*](https://github.com/adobe-fonts/source-han-sans/) and [*Source Han Serif*](https://github.com/adobe-fonts/source-han-serif/) Pan-CJK fonts as Variable Fonts, which are made available for testing purposes so that font consumers, meaning OSes, apps, layout engines, and libraries, can support such fonts. This also applies to the Google-branded [*Noto CJK*](https://github.com/googlei18n/noto-cjk/) versions. Note that the source OpenType/CFF2 fonts (aka Variable Fonts) are in the "OTF" directory, and are included only for reference purposes. The Variable Font Collections are expected to behave the same as the individual Variable Fonts.

## Supported Languages

Unlike *Source Han Sans* / *Noto Sans CJK* Version 2.000 that supports five default languages, these test fonts support a sixth. This sixth language is a third flavor of Traditional Chinese, for Macao SAR, whose regional conventions are close to those of Hong Kong SAR, but with enough differences to warrant separate fonts. We are also in the process of registering an OpenType language tag, ZHTM, for this purpose.

The six supported languages are as follows, and the two-letter region codes in parentheses are used in the font names, and also for the boxed digraph glyphs that are mapped from all 45K mappings in the fonts:

**Language** | **Two-Letter Region Code**
--- | ---
Japanese | JP
Korean (ROK) | KR
Simplified Chinese, PRC (China) | CN
Traditional Chinese, ROC (Taiwan) | TW
Traditional Chinese, Hong Kong SAR | HK
Traditional Chinese, Macao SAR | MO

The '[name](https://docs.microsoft.com/en-us/typography/opentype/spec/name)' tables include localized menu name strings that do not include the two-letter region codes, because the localized names imply the languages. Only the English-language menu name strings include the two-letter region codes.

## Design Axes

In terms of Variable Font features, the following two design axes are included:

* Weight &mdash; [wght](https://docs.microsoft.com/en-us/typography/opentype/spec/dvaraxistag_wght)
* Width &mdash; [wdth](https://docs.microsoft.com/en-us/typography/opentype/spec/dvaraxistag_wdth)

The weight range is from 200 (ExtraLight) to 900 (Heavy), and the width range is from 100% (default 1000-unit horizontal advance) to 75% (compressed).

## CFF2 Structure

Each '[CFF2](https://docs.microsoft.com/en-us/typography/opentype/spec/cff2)' table includes 65,535 glyphs (GIDs 0 through 65534). The table below indicates the glyphs that are assigned, which can vary by face:

**GIDs &amp; GID Ranges** | **Sans Serif** | **Serif**
--- | --- | ---
0 | .notdef | *same*
1 | space (mapped from U+0020 and U+00A0; aka uni0020) | *same*
2 | uni3000 (mapped from U+2003 and U+3000) | *same*
3 through 10924 | Boxed "JP" digraph | Boxed "MO" digraph
10925 through 21846 | Boxed "KR" digraph | Boxed "HK" digraph
21847 through 32768 | Boxed "CN" digraph | Boxed "TW" digraph
32769 through 43690 | Boxed "TW" digraph | Boxed "CN" digraph
43691 through 54612 | Boxed "HK" digraph | Boxed "KR" digraph
54613 through 65534 | Boxed "MO" digraph | Boxed "JP" digraph

The 'CFF2' tables have been subroutinized using the latest AFDKO *tx* tool, and are approximately 330K in size. Their unsubroutinized versions are approximately 31MB (Sans Serif) and 62MB (Serif) in size. This massive reduction in size was possible because the coverage of the six functional glyphs that represent two-letter region codes has been expanded to fill 10,922 GIDs.

The 'CFF2' tables include seven FDArray elements, and the GID assignments are as follows:

**FDArray Element** | **GID Ranges**
--- | ---
0 | 0 through 2
1 | 3 through 10924
2 | 10925 through 21846
3 | 21847 through 32768
4 | 32769 through 43690
5 | 43691 through 54612
6 | 54613 through 65534

## Unicode Mappings

The Sans Serif fonts include 44,806 mappings, and the Serif ones include 44,782, meaning 20 fewer. The 20 excluded mappings are for U+2780 &#x2780; through U+2793 &#x2793;, which correspond to Sans Serif&ndash;style characters. The 20 corresponding style-agnostic characters that are supported by both faces are U+2460 &#x2460; through U+2469 &#x2469; and U+2776 &#x2776; through U+277F &#x277F;.

The '[cmap](https://docs.microsoft.com/en-us/typography/opentype/spec/cmap)' table for each of the six languages maps the nearly 45K code points to GIDs that correspond to the boxed two-letter region digraphs. The mapping is sequential, in terms of assigning GIDs within each 10,922-glyph GID range. In other words, the 45K code points map to GIDs 3 through 10924 in the "JP" (Sans Serif) and "MO" (Serif) fonts in a sequential fashion. During the process of assigning the mappings in a sequential fashion, when GID+10924 is reached, the GID value is reset to GID+3. This process continues until all 45K code points map to a GID with the range of 10,922 GIDs. The mappings for U+0020, U+00A0, U+2003, and U+3000 are special-cased, and map to GIDs 1 or 2.

The *utf32-mappings.txt* file specifies the 44,806 code points as UTF-32 values.

## OpenType Features

The only OpenType feature that is included in the '[GSUB](https://docs.microsoft.com/en-us/typography/opentype/spec/gsub)' (*Glyph Substitution*) table is '[locl](https://docs.microsoft.com/en-us/typography/opentype/spec/features_ko#a-namelocl-idloclatag-39locl39)' (*Localized Forms*) that can be used to substitute the glyphs for the default language with those for one that is selected via language tagging in apps that support such functionality, such as Adobe Indesign and modern browsers. For example, if using the "MO" (Macao SAR) font, Adobe InDesign supports language tagging for the other five languages, meaning that it is possible to display all six digraphs together. (InDesign does not yet support language tagging for Traditional Chinese as used in Macao SAR).

## Variable Font Collections

Two of the Variable Font Collections are face-specific, meaning one for Sans Serif (aka *Source Han Sans*), and another for Serif (aka *Source Han Serif*):

* *SourceHanSansVFProto.ttc*
* *SourceHanSerifVFProto.ttc*

The following 13 'sfnt' tables are shared within each face-specific Variable Font Collection:

* BASE
* CFF2
* HVAR
* STAT
* VORG
* avar
* fvar
* hhea
* hmtx
* maxp
* post
* vhea
* vmtx

The following five 'sfnt' tables are not shared, with the number of instances indicated in parenthese:

**Table** | **Instances**
--- | ---
GSUB | 6
OS/2 | 2
cmap | 6
head | 6
name | 6

The third Variable Font Collection includes all 12 Variable Fonts, and other than doubling the number of the 'sfnt' tables that are not shared, the only difference is that there are two 'CFF2' tables:

* *SourceHanVFProto.ttc*

## How The Fonts Were Built

The 12 source Variable Fonts were built by compiling *ttx*-style XML files.

## Building the Fonts from Source

### Requirements

To build the Variable Font Collections from their Variable Font sources, you need to have installed the [Adobe Font Development Kit for OpenType](https://github.com/adobe-type-tools/afdko/) (AFDKO). The AFDKO tools are widely used for font development today, and are part of most font editor applications.

### Building the fonts

In this repository, all necessary files are included in the "OTF" directory for building the Variable Font Collections, and the *build-otc.sh* file provides the command lines that are used.

## Getting Involved

Send suggestions for changes to the Variable Font Collection Test project maintainer, [Dr. Ken Lunde](mailto:lunde@adobe.com?subject=[GitHub]%20Variable%20Font%20Collection%20Test), for consideration.
