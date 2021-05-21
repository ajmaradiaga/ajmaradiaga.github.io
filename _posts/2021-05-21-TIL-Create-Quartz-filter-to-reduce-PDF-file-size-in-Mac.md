---
layout: post
title: TIL - Create a custom Quartz filter in ColorSync utility to reduce PDF file size in Mac
---

I had a very large PDF document (18.9MB), which contained 24 large images, and I needed the file to be less than 5MB as I need to upload it on a website where the maximum file size is 5MB. Using Preview on Mac, it is possible to Export a file and select the format as PDF and Quartz filter: Reduced file size. The problem with this approach is that the image quality suffers greatly and the images in the PDF might end up not being that readable. From within Preview, it is not possible to modify the settings used by the Quartz filter. Here is where the ColorSync utility comes to the rescue :-).

In ColorSync utility, it is possible to create custom filters. What I end up doing is creating a custom filter that mimics the _Reduced File Size_ quartz filter but tweaking the setting to prevent images from loosing too much quality. with the setting below, I was able to reduce the file size from 18.9MB to 4.5MB :-). Note that the quartz filter created will then be available in Preview when exporting a PDF file.

![Quartz Filter settings]({{ site.baseurl}}/images/quartz_filter_settings.png)

### References: 
- https://www.starzsoft.com/pdf-tips/reduce-pdf-size-mac/