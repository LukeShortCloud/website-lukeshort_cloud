# Website lukeshort.cloud

## Build

```
$ git clone git@github.com:ekultails/website-lukeshort_cloud.git
$ cd website-lukeshort_cloud/
$ git submodule update --init
$ hugo serve
```

## Images

Every blog post needs a featured image. Here is the workflow for finding and using an image:

- Find a royalty free image on [Unsplash](https://unsplash.com/).
- Use [Squoosh](https://squoosh.app/) to compress and resize the image to be under 300 KiB in size.
- Move the image to `content/images/blog/<BLOG_CATEGORY>/<BLOG_DATE_ISO>-<BLOG_NAME>.jpg`.

## Customization Hints

Double the logo picture circle border from "5.5rem" to "11.0rem" for both the "width" and "height" (5.5rem to 11.0rem).

```
$ vim themes/dimension/static/assets/css/main.css
        #header .logo {
            width: 11.0rem;
            height: 11.0rem;
            line-height: 5.5rem;
            border: solid 1px #ffffff;
            border-radius: 100%;
        }
```

Double the logo picture size itself for both the "width" and the "height" (3.5rem to 7.0rem).

```
$ vim themes/dimension/static/assets/css/main.css
        .icon > img {
            display: inline-block;
            vertical-align: middle;
            max-width: 7.0rem;
            max-height: 7.0rem;
            width: auto;
            height: auto;
        }
```

The picture will not be aligned properly but will be bigger.
