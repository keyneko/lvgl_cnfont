# LVGL V9 加载中文字体教程

##### 安装[nodejs脚本](https://github.com/lvgl/lv_font_conv)

```bash
npm i lv_font_conv -g
```

```
CLI params
Common:

--bpp - bits per pixel (antialiasing).
--size - output font size (pixels).
-o, --output - output path (file or directory, depends on format).
--format - output format.
--format dump - dump glyph images and font info, useful for debug.
--format bin - dump font in binary form (as described in spec).
--format lvgl - dump font in LVGL format.
--force-fast-kern-format - always use more fast kering storage format, at cost of some size. If size difference appears, it will be displayed.
--lcd - generate bitmaps with 3x horizontal resolution, for subpixel smoothing.
--lcd-v - generate bitmaps with 3x vertical resolution, for subpixel smoothing.
--use-color-info - try to use glyph color info from font to create grayscale icons. Since gray tones are emulated via transparency, result will be good on contrast background only.
--lv-include - only with --format lvgl, set alternate path for lvgl.h.
--no-compress - disable built-in RLE compression.
--no-prefilter - disable bitmap lines filter (XOR), used to improve compression ratio.
--byte-align - pad the line ends of the bitmaps to a whole byte (requires --no-compress and --bpp != 3)
--no-kerning - drop kerning info to reduce size (not recommended).
Per font:

--font - path to font file (ttf/woff/woff2/otf). May be used multiple time for merge.
-r, --range - single glyph or range + optional mapping, belongs to previously declared --font. Can be used multiple times. Examples:
-r 0x1F450 - single value, dec or hex format.
-r 0x1F450-0x1F470 - range.
-r '0x1F450=>0xF005' - single glyph with mapping.
-r '0x1F450-0x1F470=>0xF005' - range with mapping.
-r 0x1F450 -r 0x1F451-0x1F470 - 2 ranges.
-r 0x1F450,0x1F451-0x1F470 - the same as above, but defined with single -r.
--symbols - list of characters to copy (instead of numeric format in -r).
--symbols 0123456789., - extract chars to display numbers.
--autohint-off - do not force autohinting ("light" is on by default).
--autohint-strong - use more strong autohinting (will break kerning).
Additional debug options:

--full-info - don't shorten 'font_info.json' (include pixels data).
```




##### 使用示例

```bash
lv_font_conv --font NotoSerifSC_Regular.ttf --size 14 --format lvgl --bpp 1 -o ./NotoSerifSC_Regular.c --range 0x20-0x7F,0x4E00-0x9FFF

lv_font_conv --font NotoSerifSC_Regular.ttf --size 14 --format lvgl --bpp 1 -o ./NotoSerifSC_Regular.c --symbols "0123456789你好，世界！"
```



## 在lvgl中使用字体 

把生成的.c文件拷贝到工程目录下，lv_conf.h中定义字体

```c
/*Optionally declare custom fonts here.
 *You can use these fonts as default font too and they will be available globally.
 *E.g. #define LV_FONT_CUSTOM_DECLARE   LV_FONT_DECLARE(my_font_1) LV_FONT_DECLARE(my_font_2)*/
#define LV_FONT_CUSTOM_DECLARE LV_FONT_DECLARE(NotoSansSC_Regular)

```



在main.c中使用字体 

```c
/*Create a label to display strings*/
  lv_obj_t* label = lv_label_create(lv_screen_active());
  lv_obj_set_style_text_font(label, &NotoSansSC_Regular, 0);
  lv_label_set_text(label, "你好，世界！");
  lv_obj_align(label, LV_ALIGN_CENTER, 0, 0);

```

