# Collection Report & Guide 

| S.No | Title | Summary | Link | Tools |
| ---- | ----- | ------- | ---- | ----- |
| 1    | Stored XSS in SVG file as data: url | In rich text editors (products, gifts, pages, etc) you allow data: URLs to be set as image sources, and I was able to store XSS in such image. While <img> won't execute script that is stored inside the SVG it points to, if one opens the image, the script will be executed. What is important here is that you actually can use javascript here to access /admin area. | [HackerOne](https://hackerone.com/reports/1276742) |   -    |