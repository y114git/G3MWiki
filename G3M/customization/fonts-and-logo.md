# Fonts and logo

**Settings > Appearance > Graphics** selects a custom font and a main logo image. Their controls offer removal when a custom file is active.

## Font

Custom font formats are TrueType `.ttf` and OpenType `.otf`. G3M loads the selected font for its interface. Text still depends on whether that font contains the characters used by the selected language.

A font with Latin letters alone is not sufficient for Japanese, Korean, or Chinese interface text. Test your theme in the languages you intend it to support.

Theme export includes the active custom font as `custom_font.ttf` or `custom_font.otf`. Check its redistribution license before sharing it.

## Logo

The logo replaces the main interface logo. Use a readable image with appropriate empty space around its content. Usable image formats include PNG, JPEG, GIF, BMP, ICO, and WebP.

A theme package calls the image `custom_logo.<extension>`. The catalog can use a public URL to that image as the theme's icon, but the package and catalog icon are separate choices.

## Border radius

**Border Radius** controls corner rounding, from 0 to 999 pixels in the settings interface. A value of 0 gives square corners. The rendered radius is limited by the size of each control.

Corner rounding does not change the outline's thickness. The standard theme-export field is `custom_border_radius`.
