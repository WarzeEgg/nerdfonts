# Inconsolata

Open-source monospace font for code listings, originally by [**@raphlinus**](https://github.com/raphlinus/)

### Ligatures

Inconsolata includes ligatures for a few JavaScript operators:

They are available in two families.

- **"Inconsolata"** exposes the ligatures as `dlig`.  These are disabled by default, and probably won't show up in your editor.  You can enable them in CSS with this rule:
   ```css
  font-variant-ligatures: discretionary-ligatures;
  ```
- **"Ligconsolata"** exposes the ligatures as `liga`.  These are enabled by default.  This is the family you should use in your text editor.

Note: the Ligconsolata variant has not yet been upgraded to version 3.000, as we're prioritizing the non-ligature variants.

## Building the family

Family is built using Glyphs, fontmake and gftools post processing script. Tools are all python based.

To install all the Python tools into a virtualenv, do the following:

```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

To build the fonts we must load sources/Inconsolata-vf.glyphs in Glyphs and do the following:
- Run the decompose-transformed-components.py script
- Run the gen_instances.py script
- Run the inco_fix.py script
- Save the file back in the sources directory with the filename "prod.glyphs"

We can now run the build script in the terminal:

```
cd sources # script needs to be run from sources dir
sh build.sh
```

Fonts will take approximately 25 minutes to build.

## Changelog v.3.000

Upgrade to 2-axis variable font family, with widths from 50 to 200, and weights from 200 to 900.

## Changelog v.2.013

- Removed ligatures for `fi` and `fl`.
- Operator ligatures moved to `dlig`.
- New variant "Ligconsolata" introduced, which exposes operator ligatures as `liga`.

## Changelog v.2.011

March 2018 glyph set expansion was completed by [**@appsforartists**](https://github.com/appsforartists/), which included:

- [x] Glyph Set expanded to include ligatures for ===, !==, =>, <=, >=, ->, <-

## Changelog v.2.001

August 2016 glyph set expansion was completed by Alexei Vanyashin ( [Cyreal][5] ), which included:

- [x] Glyph Set expanded to GF Latin Pro
- [x] Additional glyphs ⊕↑↗→↘↓↙←↖↔↕⇧⇨⇩⇦⬆⮕⬇⬅●○◆◇☹☺☻♠♣♥♦✓✔✕✗✘␣⎋⌂⇪⌧⌫⌦⌥⌘⏎�
- [x] Minor design improvements (trademark corner spurs)

Further reading: Inconsolata expansion project thread on [Google Fonts Discussions][6]

#### Supported glyphs sets:

* GF Latin Pro

![Inconsolata Preview](documentation/img/inco-preview.png)

## License

This Font Software is licensed under the SIL Open Font License, Version 1.1.
This license is copied below, and is also available with a FAQ at:
[http://scripts.sil.org/OFL][4]

----

## Inconsolata Build Instructions

Inconsolata fonts can be built using either export from Glyphs or using [fontmake]. The font files committed to this repo are done using fontmake.

### Source Files

Inconsolata source files are available in `.glyphs` format located in the `/sources` directory.

### Adding ligatures

1. Follow the ["Creating the ligature"](https://glyphsapp.com/tutorials/ligatures) section of the Glyphs ligatures tutorial.
2. Name your new glyph with the suffix `.dlig`, for instance `bar_greater.dlig`.
3. Open the _Font Info_ panel.
   1. Switch to the _Features_ tab.
   2. Click _dlig_ in the sidebar.
   3. Click the _Update_ button at the bottom of the panel.
   4. Switch to the _Instances_ tab.
   5. Update the _Rename Glyphs_ value for "Ligconsolata Regular" to include a new line for your new glyph, for instance:
      ```
      bar_greater.dlig=bar_greater.liga
      ```
   6. Update the _Rename Glyphs_ value for "Ligconsolata Bold".
4. Export the font, as explained below.

### Exporting a variable font using fontmake

It's possible to export the project as a single variable font. It's just a bit tricky, because the font uses components with varying 2x2 components, triggering a [bug](https://github.com/googlefonts/fontmake/issues/595) which is present in both [fontmake] and Glyphs export. Thus, there's an inco_fix.py script in the sources directory that detects this case and decomposes just those components. Run that script before exporting. The script also decomposes corner components, which makes the resulting `glyphs` file suitable for fontmake export as well (fontmake currently has no support for corner components).

You can copy the script into the Scripts folder for Glyphs, which will make it available in the Script menu, or you can just copy it into the Macro Panel.

After running the script, the following fontmake invocation will generate a variable font:

```
fontmake -g sources/Inconsolata-vf.glyphs -o variable
```

This is the version in the fonts/ directory, as it is slightly smaller than the version generated by Glyphs.

We do not check the *result* of the inco_fix script into version control, as we want to preserve editability. It's entirely possible that a future version of fontmake (or Glyphs itself) will be able to handle the source file without running a script.

### Exporting instances using fontmake

The source file contains 15 instances, including all weights of the normal (100) width, and also all masters. This is a reasonable complement for working on the font. Run the gen_instances.py script to generate a total of 72 instances; all combinations of the weights from 200 to 900, and widths 50, 70, 80, 90, 110, 120, 150, and 200.

There are two other instances for Ligconsolata, and fontmake will attempt to generate those, but the "Rename Glyphs" custom parameter doesn't seem to be respected by fontmake, so these won't have ligatures enabled. Use the Glyphs export instead (detailed below).

Then run this command to generate OTF:

```
fontmake -g sources/Inconsolata-vf.glyphs -i -o otf
```

And this command to generate autohinted TTF:

```
fontmake -g sources/Inconsolata-vf.glyphs -i -o ttf -a
```

These are the versions in the fonts/ directory.

### Font Export options (from Glyphs)

This is the preferred way to generate the Ligconsolata instances, but

* TTF and OTF files should be exported into `/fonts/ttf` and `/fonts/otf` folders.

* `TTFs` should be generated from Glyphs App with `Autohint` option checked. At the moment there is no custom build script required to produce font files, since default TTFautohinting options suffice.

![Font Export Options](documentation/font-export.png)

* `OTFs` should be generated with these options:
  * [X] Remove Overlap
  * [X] Autohint
  * [ ] Save as TTF
  * [X] Export destination: $REPO_PATH/fonts/otf

### Future work

In addition, we want to export a subset not including Vietnamese script coverage, to avoid over-large line spacing on older applications (such as terminals and text editors) that don't understand the "use typo metrics" flag (see https://github.com/googlefonts/Inconsolata/issues/35).

## Glyphstool

The repository also contains some Rust code to manipulate Glyphs format masters, in the `glyphstool` subdirectory. This was used to apply global transforms (mostly as a starting point for the width work). Perhaps the most valuable aspect is that it contains a fairly complete set of line and box drawing primitives, inspired by [Source Code Pro] but with actually variable weight and width. It's not particularly polished or well documented, but is provided for completeness, and it's possible that it could be adapted to future tools that work with font data in the Glyphs format. The code is licensed under Apache 2.0 or MIT, in keeping with the Rust tradition.

----

## Copyright

Copyright 2006 The Inconsolata Project Authors

## Links

* [Inconsolata on Google Fonts][1]
* [Inconsolata on Levien.com][2]
* [Official Upstream on git][3]

[1]: https://fonts.google.com/specimen/Inconsolata
[2]: http://levien.com/type/myfonts/inconsolata.html
[3]: https://github.com/google/fonts/tree/master/ofl/inconsolata
[4]: http://scripts.sil.org/OFL
[5]: http://cyreal.org
[6]: https://groups.google.com/forum/#!searchin/googlefonts-discuss/inconsolata%7Csort:relevance/googlefonts-discuss/wgVuOx9yo5k/2QSUQ78CCQAJ
[fontmake]: https://github.com/googlefonts/fontmake

## Which font?

### TL;DR

* Pick your font family and then select from the `'complete'` directory.
  * If you are on Windows pick a font with the `'Windows Compatible'` suffix.
    * This includes specific tweaks to ensure the font works on Windows, in particular monospace identification and font name length limitations
  * If you are limited to monospaced fonts (because of your terminal, etc) then pick a font with the `'Mono'` suffix.
    * This denotes that the Nerd Font glyphs will be monospaced not necessarily that the entire font will be monospaced

### Ligatures

By the *Nerd Font* policy, the variant with the `'Mono'` suffix is not supposed to have any ligatures.
Use the non-*Mono* variants to have ligatures.

### Explanation

Once you narrow down your font choice of family (`Droid Sans`, `Inconsolata`, etc) and style (`bold`, `italic`, etc) you have 2 main choices:

#### `Option 1: Download already patched font`

 * download an already patched font from the `complete` folder
   * This is most likely the one you want. It includes **all** of the glyphs from all of the glyph sets. Only caution here is that some fonts have glyphs in the _same_ code point so to include everything some had to be moved to alternate code points.

#### `Option 2: Patch your own font`

 * patch your own variations with the various options provided by the font patcher (see each font's readme for full list of combinations available)
   * This is the option you want if the font you use is _not_ already included or you want maximum control of what's included
   * This contains a list of _all permutations_ of the various glyphs. E.g. You want the font with only [Octicons][octicons] or you want the font with just [Font Awesome][font-awesome] and [Devicons][vorillaz-devicons]. The goal is to provide every combination possible in this folder.


For more information see: [The FAQ](https://github.com/ryanoasis/nerd-fonts/wiki/FAQ-and-Troubleshooting#which-font)


[vim-devicons]:https://github.com/ryanoasis/vim-devicons
[vorillaz-devicons]:https://vorillaz.github.io/devicons/
[font-awesome]:https://github.com/FortAwesome/Font-Awesome
[octicons]:https://github.com/primer/octicons
[gabrielelana-pomicons]:https://github.com/gabrielelana/pomicons
[Seti-UI]:https://atom.io/themes/seti-ui
[ryanoasis-powerline-extra-symbols]:https://github.com/ryanoasis/powerline-extra-symbols
[SIL-RFN]:http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web_fonts_and_RFNs#14cbfd4a
