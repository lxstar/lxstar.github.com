---
layout: post
title:  "simple-lightbox 添加exif信息"
tags: [wordpress, simple-lightbox]
---

Summary: simple-lightbox 添加exif信息


<!--summary-->

simple-lightbox是一个图片显示的wordpress控件，缺少exif信息。

查阅官方git发现这个功能是一个add-on还是open的，不知道什么时候会加上

自己加上把

---

在simple-lightbox的目录下functions.php末尾添加

{% highlight php %}
function get_image_info($img) {
    $exif = exif_read_data(''.$img.'', 0, true);
    $exif_out = '器材: '.$exif[IFD0][Make].$exif[IFD0][Model]
                .' 光圈: '.$exif[COMPUTED][ApertureFNumber]
                .' 快门: '.$exif[EXIF][ExposureTime]
                .' 焦距: '.$exif[EXIF][FocalLength].'mm'
                .' ISO: '.$exif[EXIF][ISOSpeedRatings]
                .' 拍摄日期: '.$exif[EXIF][DateTimeOriginal];
    return $exif_out;
}

function add_exif_info($item, $key) {
    return (object)array_merge((array)$item, array('exif' => get_image_info($key)));
}
{% endhighlight %}

在simple-lightbox的目录下controller.php中修改876-878行

{% highlight php %}

// 修改前
foreach ( $this->media_items as $key => $props ) {
    $this->media_items[$key] =  $this->util->apply_filters('media_item_properties', (object) $props);
}

// 修改后
foreach ( $this->media_items as $key => $props ) {
    $this->media_items[$key] =  $this->util->apply_filters('media_item_properties', (object) $props);
    $this->media_items[$key] = add_exif_info($this->media_items[$key], $key);
}

{% endhighlight %}

再继续添加显示的地方 simple-lightbox/themes/baseline/layout.html 27-42行

添加 \{\{item.exif\}\}

![效果图]({{ site.url }}/static/images/code_html_simple-lightbox.png)

这样就可以ok了，效果如下：

![效果图]({{ site.url }}/static/images/exif-add-example.jpg)

---
最后吐槽一下，simple-lightbox的代码写的确实不错

可是一个插件有必要上JS的重框架吗，template-tag看的都吐了

就是写完不想让人改的节奏，不能一起玩耍了。。。