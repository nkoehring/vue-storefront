# Announcements Plugin for VuePress

This plugin injects a global component into a VuePress page which uses a special frontmatter attribute to show announcement popups.

## Usage

Add the announcement plugin to plugins section in `.vuepress/config.js`:

```js
// .vuepress/config.js
module.exports = {
  plugins: [
    'vuepress-announcements'
  ]
}
```

There are no additional options.

To show announcements, add an `announcements` list to the page frontmatter. Here is a full example:

```md
---
title: Meet Vue Storefront
announcements:
  - title: Important bold title text
    content: This text will be shown next to the bold title text.
    activeFrom: 2022-02-15
    activeTill: 2022-02-25
    actions:
      - title: Go Home
        href: /
  - title: Did you know?
    content: Announcements are only shown between activeFrom and activeTill. Actions are translated into router-links.
    activeFrom: 2022-02-18
    activeTill: 2022-02-28
```

This example shows an announcement titled "Important bold title text" from 15th till 25th of February and a second notification from 18th till 28th of February titled "Did you know?". The action in the first will be a `<router-link>` "Go home". 

The `activeFrom` and `activeTill` parameter can be any string that Date.parse can handle. ISO date strings is recommended for compatibility.

 > Attention: without timezone information, the timestamp will assumed to be in UTC!

| Parameter  |        Default       |    Details    |
|------------|----------------------|---------------|
| title      | "" (empty string)    | String, shown in bold |
| content    | "" (empty string)    | String        |
| activeFrom | required, no default | DateTime, Announcement will not be shown before this date |
| activeTill | required, no default | DateTime, Announcement will not be shown after this date  |
| actions    | optional             | List of { title, href } |

## TODO

`vuepress-announcements` is in a very early stage. It lacks features like themeability and doesn't handle page changes in a good way. An option to show global instead of per-page announcements might be desirable as well.
