# feta - edit image metadata with `feh`

[feh][feh] + [exiftool][exiftool] + [rofi][rofi] to edit image meta data.

## What?

It's just a BASH script that calls `feh` and sets some actions. The actions use
`rofi` for user input and `exiftool` to change the metadata on the images.

By default it sets [XMP][xmp] tags in the [Dublin Core][dc] [namespace][dcns]

### Actions
1. **Set Subjects**: Replace all subject tags with selection or entry
2. **Add Subjects**: Add subject tags based on selection or entry
3. **Rm Subjects**: Remove tags based on selection
4. **Set Title**:  Set title based on entry
5. **Set Description**:  Set description based on entry

## Why?

I wanted to add metadata to a bunch of images. I didn't want to install a big
bloated photo management GUI. I already had `feh`, `exiftool`, and `dmenu`
(actually `rofi`, because I wanted the `-multi-select` option for subjects).
Those tools are great and can do all the things.

It sets XMP-dc tags as default because afaik that's hip, cool, open, modern
standard or something. This isn't my wheelhouse tho, so tell me if there's a
better way.

## Install
Install `feh`, `exiftool`, and `rofi`...probably just use your package manager.

Put the script `bin/feta` in your `$PATH`

## Usage

```sh
$ feta path/to/images
```

You could call the sub functions directly from the command line if you wanted
to, but you might as well just use `exiftool` or whatever directly.

## Config

By default, it looks for a config file in `$XDG_CONFIG_HOME/feta.cfg`. That
can be overridden by setting `$FETA_CONFIG`.

Environment variables override config values.

| config var     | env var             | default                        |
|----------------|---------------------|--------------------------------|
| `subject_list` | `FETA_SUBJECT_LIST` | `$XDG_DATA_HOME/feta-subjects` |
| `tag_subject`  | `FETA_SUBJECT`      | `XMP-dc:subject`               |
| `tag_title`    | `FETA_TITLE`        | `XMP-dc:title`                 |
| `tag_desc`     | `FETA_DESC`         | `XMP-dc:description`           |
| `exiftool_opt` | n/a                 | `()`                           |

- `subject_list` is the path to a text file of available "subjects." Subject
  strings will be added to the file (if it exists) when using the interface.

- `tag_*` options set what tags `exiftool` actually sets.

- `exiftool_opt` is an array of options passed to `exiftool` when setting
  metadata. By default, `exiftool` makes copies of original images so you don't
  lose data. If you're feeling lucky you could set eg.
  `exiftool_opt=(-overwrite_original)` in the config to suppress this behavior.

### `feh` Theme

If you don't like the options `feta` sets when calling `feh`, you can use `feh`
themes (see [feh documentation][fehdocs]) and set it up yourself.

Basically, if `feta` is in your `$PATH`, you `symlink` `feh` into your path as
eg. `~/bin/gouda`, and setup a `feh` theme (`~/.config/feh/themes`) as the
following...you should get the basic `feta` functionality by calling `gouda`
without some of the "unnecessary" settings that are default. Read the `feh`
docs, look at the `run` function in `feta` and tweak to your liking.

```
gouda --info "feta info %F" --action1 ";[Set Subjects] feta prompt-set-subjects %F" --action2 ";[Add Subjects] feta prompt-add-subjects %F" --action3 ";[Rm Subjects] feta prompt-rm-subjects %F" --action4 ";[Set Title] feta prompt-set-title %F" --action5 ";[Set Desc] feta prompt-set-desc %F"
```


[feh]: https://feh.finalrewind.org/
[exiftool]: https://exiftool.org/
[rofi]: https://github.com/davatorium/rofi
[dc]: https://dublincore.org/
[xmp]: https://www.adobe.com/products/xmp.html
[dcns]: https://exiftool.org/TagNames/XMP.html#dc
[fehdocs]: https://man.finalrewind.org/1/feh/
