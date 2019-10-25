# heroku-buildpack-emoji-support

Heroku buildpack that add native support for emojis on a Heroku-18 stack. This buildpack can be forked to install various fonts as well, just by adding them in a `fonts` folder and update `fonts.tar.gz` with command 
```
tar -czvf fonts.tar.gz fonts
```

# Installation

You just need to [add this buildpack](https://devcenter.heroku.com/articles/buildpacks#setting-a-buildpack-on-an-application) to the app's buildpack list, prior any buildpack.

# What it does

It seems that the Ubuntu distribution used by the Heroku-18 stack comes without any fonts installed besides `DejaVu`, that supports a limited fraction of emojis. What this buildpack simply does is adding custom fonts in the `~/.fonts` folder and refresh the font cache. 
The custom fonts included in this Buildpack are `NotoEmoji-Regular.ttf` and `NotoColorEmoji.ttf`, as Ubuntu 18.04 LTS supports natively the Noto Color Emoji font.

# Limitations
As said earlier, `DejaVu` comes with a limited range of black and white emojis, that takes precedence to `NotoColorEmoji`. The buildpack adds a fontconfig to limit this precedence (for instance, the 🍪emoji), but most of them are overrided.

The provided fontconfig comes from this repo https://github.com/stove-panini/fontconfig-emoji. More informations can be found there. The one that is applied is in the `fontconfig` folder, the others are in the `extra-fontconfig` one.

You can easly add more fontconfigs by adding them in the folder `fontconfig`.

This issue is largely discussed over the net, but without a real and clean solution :
- https://bugs.webkit.org/show_bug.cgi?id=191976#c1
- https://gitlab.freedesktop.org/fontconfig/fontconfig/issues/136
- https://www.reddit.com/r/archlinux/comments/9ouk2o/how_to_priorize_emojione_noto_emoji_over/

The "best" way would be to apply `70-no-dejavu.conf` and replace `DejaVu` with `Bitstream Vera`, but it could lead to other unwanted side effects, so... Kinda stuck here.

## Original inspirations
- https://github.com/debitoor/heroku-buildpack-converter-fonts
- https://github.com/stove-panini/fontconfig-emoji
