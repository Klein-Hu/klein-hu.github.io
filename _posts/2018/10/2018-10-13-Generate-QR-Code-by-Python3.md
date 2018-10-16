---
layout: post
title: Generate QR Code with Python 3
categories: [Tips, Python, QR Code]
tags: [Family, Python]
fullview: false
comments: true
---

It is the time for my daughter to sell some stuff for school fundraiser. To make the process smooth, I suggest her to generate some QR Code for the online store address and pass them to neighbors. We coud do it online for sure. But maybe we could do it locally with Python?

Install [qrcode](https://pypi.org/project/qrcode/) and [Pillow](https://pypi.org/project/Pillow/)

## Using `qr` script

Something like 

```bash
qr "blahblah" > test.png
```

## Using Simple Script

In Python script, save the output `PNG`.

```Python
import qrcode
img = qrcode.make('Some data here')
img.save("image1.png")
```

## Advanced Usage

In Python script, 

```Python
import qrcode

qr = qrcode.QRCode(
    version = 2,
    error_correction = qrcode.constants.ERROR_CORRECT_H,
    box_size = 10,
    border = 4,
)

data = "此处是中文"
qr.add_data(data)
qr.make(fit=True)

img = qr.make_image()
img.save("image1.png")
```