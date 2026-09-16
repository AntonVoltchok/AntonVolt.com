# Custom WordPress theme with AI + ACF (no page builder)

Follow-along guide for the second half of Ferdy Korpershoek’s video **[I Built a Custom WordPress Theme With AI (No Page Builder)](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=1373s)**.

Start at **22:00**, which is the end of **Publish your website and go live**. The WordPress work begins at **22:53**.

| Chapter | Time | What you do |
| --- | --- | --- |
| Get Novamira | [22:53](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=1373s) | Put an AI agent inside WordPress |
| Get ACF | [26:13](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=1573s) | Install Advanced Custom Fields |
| Turn your website into a WordPress theme | [31:24](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=1884s) | Convert the HTML site into a classic PHP theme |
| Optimize your website for Google | [38:23](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=2303s) | Titles, meta, sitemap, crawlability |
| Add Google Analytics | [42:20](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=2540s) | GA4 measurement ID |
| Add Google Search Console | [45:17](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=2717s) | Verify the property and submit a sitemap |
| Assign user roles | [47:01](https://www.youtube.com/watch?v=dXyjzDFSKaw&t=2821s) | Give clients only the fields they should edit |

Video (57:22): [youtube.com/watch?v=dXyjzDFSKaw](https://www.youtube.com/watch?v=dXyjzDFSKaw)

## How to use this file

This is a **working guide**, not a word-for-word transcript.

YouTube blocked playback and captions from this environment, so there are **no screenshots**. Unreadable or face/WaveMakers stills would not have helped anyway. Jump the video with the timestamps above while you follow the steps here.

The chapter titles, description, and tool links below come from the video page. The install/config steps come from the current **NovaMira** and **ACF** docs, which are the products that chapter is about.

**Point of the workflow:** keep the design in the theme. Clients edit named ACF fields (and custom post types such as events, sermons, speakers). They never get a page builder they can break.

---

## Where you should be at 22:00

The first 22 minutes are Claude Design → Claude Code → a live **HTML** site. Skip that, and skip any WaveMakers marketing page.

You want these on disk before you touch WordPress:

- The exported HTML/CSS/JS (and images/fonts)
- A local or staging WordPress install, **not production**
- WordPress **6.9+** and PHP **8.0+** (NovaMira needs the Abilities API in core)
- Git on the theme folder so you can revert bad agent edits

A typical local stack: Local WP, DDEV, or `wp-env`.

---

## 1. Get NovaMira — 22:53

NovaMira is a WordPress plugin that connects Claude, Cursor, Codex, etc. **directly to your site** over MCP. The agent can run PHP, read/write theme files, query the database, and install plugins, instead of you pasting snippets back and forth.

Download: [ferdy.com/novamira](https://ferdy.com/novamira) or [novamira.ai](https://novamira.ai/)  
Docs: [Getting started](https://novamira.ai/docs/getting-started/)

Use it on **dev/staging only**, with a backup. NovaMira does not roll back changes.

### Install

1. Download the ZIP from the NovaMira download page.
2. In wp-admin: **Plugins → Add Plugin → Upload Plugin**.
3. Install and **Activate**.
4. Confirm a **Novamira** item appears in the admin sidebar.

On activation it bundles the MCP Adapter and registers abilities (Execute PHP, Read/Write/Edit/Delete/Disable/Enable File, List Directory, Create Upload Link). **AI abilities stay off** until you enable them.

If you already have the standalone MCP Adapter plugin active, deactivate that copy so it does not conflict.

### Turn abilities on

1. **Novamira → Configuration** (or Settings).
2. Check **Enable AI Abilities** and confirm the warning.
3. Save.

You should see a red **Novamira ON** indicator in the admin bar. If that ever appears on production, turn it off immediately.

### Connect Cursor or Claude Code

The Configuration screen is built around the client you pick. It fills in **your** site URL. Prefer that over typing this by hand.

**Cursor** — add to `~/.cursor/mcp.json` or the project `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "novamira-your-site": {
      "url": "https://your-site.com/wp-json/mcp/novamira-oauth"
    }
  }
}
```

Use your real HTTPS URL. Local sites often need the `mcp-remote` bridge shown on the same admin page.

**Claude Code:**

```bash
claude mcp add novamira-your-site --transport http https://your-site.com/wp-json/mcp/novamira-oauth
```

Then sign in in the browser from your WordPress login. Revoke later under **Novamira → Manage Connections**.

OAuth needs HTTPS (or a local environment). If the host filters cloud AI traffic, use an **Application Password** instead (endpoint `/wp-json/mcp/novamira`, no `-oauth` suffix). Copy the generated prompt from the Connect page rather than emailing that password around.

### Prove it works

New agent chat. Ask:

> List the plugins installed on this WordPress site.

You should see Discover / Get / Execute Ability tools, then a real plugin list. Keep **require approval** on for MCP tools.

---

## 2. Get ACF — 26:13

Install **Advanced Custom Fields** so the theme can expose structured fields instead of Gutenberg soup or Elementor.

1. **Plugins → Add Plugin**.
2. Search **Advanced Custom Fields**.
3. Install **Advanced Custom Fields** by WP Engine and activate.

You should get an **ACF** admin menu: Field Groups, Post Types, Taxonomies.

Free ACF is enough for text, images, WYSIWYG, and basic groups. **Repeaters, Flexible Content, and Options pages** need **ACF PRO**. If the homepage is a stack of repeating sections (speakers, events, FAQs), Pro (or a NovaMira Pro ACF skill) will save a fight.

Why ACF in this video: clients get a form that matches the design. They do not get a canvas that can destroy the layout.

---

## 3. Turn the HTML site into a WordPress theme — 31:24

This is the core chapter. The agent, through NovaMira, should rebuild your static site as a **classic PHP theme** under `wp-content/themes/your-theme/`.

### Minimum theme files

```text
wp-content/themes/your-theme/
  style.css          required header + your CSS (or import it)
  functions.php      enqueue, menus, theme supports, ACF JSON
  index.php          fallback
  header.php
  footer.php
  front-page.php     homepage markup from the HTML
  page.php           interior pages
  single.php         one event / sermon / speaker
  archive.php        lists
  screenshot.png     optional, shows in Appearance → Themes
  assets/            css, js, images, fonts from the HTML export
```

`style.css` must start with a theme header or WordPress will not list the theme:

```css
/*
Theme Name: Your Theme Name
Author: Your Name
Description: Custom theme converted from the HTML site. Editable content via ACF.
Version: 1.0.0
Requires at least: 6.9
Requires PHP: 8.0
Text Domain: your-theme
*/
```

Activate it under **Appearance → Themes**.

### What to tell the agent

Point it at the HTML export **and** the live WordPress site (NovaMira). Something like:

> Convert the HTML site in [path] into a classic WordPress theme named [name] in wp-content/themes/[name].
>
> - Do not use Elementor, Bricks, Divi, or the site editor as the layout engine.
> - Split shared chrome into header.php and footer.php. Call wp_head() and wp_footer().
> - Enqueue the existing CSS/JS with wp_enqueue_style / wp_enqueue_script. Do not dump a second copy of the CSS in style.css besides the theme header.
> - Register a Primary menu and output it with wp_nav_menu().
> - Front page: front-page.php using the homepage sections from the HTML.
> - Replace hardcoded copy and images with ACF fields (see next section).
> - Escape output. Use esc_html, esc_url, esc_attr, wp_kses_post.
> - After writing files, activate the theme and tell me which templates map to which URLs.

Let it work in small slices (header/footer first, then homepage, then inner templates). Approve file writes one batch at a time.

### Wire WordPress into the HTML

In `header.php`:

- `language_attributes()` on `<html>`
- `bloginfo('charset')` and `wp_head()` in `<head>`
- `body_class()` on `<body>`

In `footer.php`: `wp_footer()` before `</body>`.

In `functions.php`:

```php
<?php
add_action('after_setup_theme', function () {
    add_theme_support('title-tag');
    add_theme_support('post-thumbnails');
    add_theme_support('html5', ['search-form', 'gallery', 'caption', 'style', 'script']);
    register_nav_menus([
        'primary' => 'Primary Menu',
        'footer'  => 'Footer Menu',
    ]);
});

add_action('wp_enqueue_scripts', function () {
    $ver = wp_get_theme()->get('Version');
    wp_enqueue_style('theme-main', get_stylesheet_directory_uri() . '/assets/css/main.css', [], $ver);
    wp_enqueue_script('theme-main', get_stylesheet_directory_uri() . '/assets/js/main.js', [], $ver, true);
});
```

Adjust paths to match the export.

### Custom post types (events, sermons, speakers)

The video description calls out **events, sermons**, and similar objects. In ACF:

1. **ACF → Post Types → Add New**.
2. Plural / singular labels, e.g. Events / Event.
3. Post Type Key: `event` (lowercase, ≤ 20 chars).
4. Public: on. Enable archive if you want `/events/`.
5. Supports: Title, Featured Image. Turn off editor if all body copy is ACF.
6. Save, then **Add fields to** this post type.

Repeat for `sermon` and `speaker` if those sections exist in the HTML.

Or ask the connected agent:

> Create public CPTs event, sermon, and speaker with archives. Then create ACF field groups that appear only on those types.

NovaMira Pro can drive ACF field groups, CPTs, taxonomies, options pages, and values from chat. Free NovaMira can still write PHP and files; you can also click the ACF UI yourself.

### ACF field groups for the pages you already designed

**ACF → Field Groups → Add New**.

For each HTML section, add fields that a client can understand:

| HTML section | Typical fields |
| --- | --- |
| Hero | heading (text), subheading (textarea), background (image, return ID), CTA (link) |
| About / intro | heading, body (WYSIWYG) |
| Events list | sourced from the Event CPT, not a giant WYSIWYG |
| Speakers | repeater or Speaker CPT: name, role, photo, bio |
| Contact | address, phone, email, embed URL |

Location rules examples:

- Homepage group → Page type is **Front Page**
- Event group → Post Type is **Event**
- Shared header/footer bits → Options page (ACF PRO) or a “Site settings” page

Field **label** is for humans. Field **name** is for PHP (`hero_heading`, no spaces).

### Output fields in the theme

```php
<?php
$heading = get_field('hero_heading');
$image_id = get_field('hero_image'); // return format: Image ID
?>

<section class="hero">
  <?php if ($heading) : ?>
    <h1><?php echo esc_html($heading); ?></h1>
  <?php endif; ?>

  <?php
  echo wp_get_attachment_image(
      (int) $image_id,
      'full',
      false,
      ['class' => 'hero__image']
  );
  ?>
</section>
```

Repeaters:

```php
<?php if (have_rows('speakers')) : ?>
  <ul class="speakers">
    <?php while (have_rows('speakers')) : the_row(); ?>
      <li><?php echo esc_html(get_sub_field('name')); ?></li>
    <?php endwhile; ?>
  </ul>
<?php endif; ?>
```

Keep the original class names from the HTML so the CSS still matches.

### ACF Local JSON (do this once)

In `functions.php`:

```php
add_filter('acf/settings/save_json', function () {
    $dir = get_stylesheet_directory() . '/acf-json';
    if (!is_dir($dir)) {
        wp_mkdir_p($dir);
    }
    return $dir;
});

add_filter('acf/settings/load_json', function ($paths) {
    $paths[] = get_stylesheet_directory() . '/acf-json';
    return $paths;
});
```

Field groups then live in git as JSON. Sync them from **ACF → Field Groups** on the next environment.

### Sanity checks before leaving this chapter

- [ ] Theme is active and the homepage matches the HTML layout
- [ ] CSS/JS still load (view source: `wp-content/themes/...`)
- [ ] Menu is a WordPress menu, not hardcoded `<a>` tags only
- [ ] Homepage copy/images come from ACF, not only from the PHP file
- [ ] `/event/something/` uses `single-event.php` or `single.php`
- [ ] No page builder plugin required to change a heading

---

## 4. Optimize for Google — 38:23

You already have `title-tag` support. Finish the boring SEO that actually matters:

1. **Settings → Permalinks** → Post name. Save.
2. One **H1** per template, from `the_title()` or the ACF hero heading — not both fighting.
3. Unique title + meta description per URL. A dedicated SEO plugin (Rank Math, Yoast, The SEO Framework) is fine; do not install two.
4. Image **alt** from the media library / ACF image field.
5. XML sitemap (the SEO plugin, or `wp-sitemap.xml` in modern WordPress).
6. `robots.txt` should not `Disallow: /` on the live site.

Ask the agent only after the theme renders correctly:

> Add semantic headings, image alt from ACF, and a unique document title on front-page.php and CPT singles. Do not install a second SEO plugin if one is already active.

---

## 5. Add Google Analytics — 42:20

1. Create a GA4 property at [analytics.google.com](https://analytics.google.com/).
2. Copy the Measurement ID (`G-XXXXXXXX`).

Lightest theme-only install:

```php
add_action('wp_head', function () {
    $id = 'G-XXXXXXXX';
    ?>
    <script async src="https://www.googletagmanager.com/gtag/js?id=<?php echo esc_attr($id); ?>"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', '<?php echo esc_js($id); ?>');
    </script>
    <?php
}, 1);
```

Alternatively **Site Kit by Google** covers Analytics and Search Console in wp-admin. Do not double-fire gtag (plugin + hardcoded snippet).

Confirm in GA4 **Realtime** with the site open.

---

## 6. Add Google Search Console — 45:17

1. [search.google.com/search-console](https://search.google.com/search-console) → Add property (URL prefix is enough).
2. Verify with the HTML tag, DNS TXT, or Site Kit if you used it for Analytics.
3. Sitemaps → submit `https://your-domain.com/sitemap_index.xml` or `/wp-sitemap.xml`.
4. Inspect the homepage → Request indexing.

Keep the verification tag in the theme or via the plugin so a theme tweak does not unverified you.

---

## 7. Assign user roles — 47:01

This chapter is long (~10 minutes) because the whole point is a **client-safe admin**.

Default roles:

| Role | Use here |
| --- | --- |
| Administrator | You. Themes, plugins, NovaMira, ACF field groups. |
| Editor | Too much (pages, others’ content, often themes depending on extras). |
| Author / Contributor | Weak fit for ACF landing pages. |
| Subscriber | Front-end only. Useless for editing ACF. |

What you want instead: a role that can **edit the posts/pages/CPTs you wired to ACF**, upload media, and nothing else — no **Appearance**, **Plugins**, **Settings**, **Novamira**, no field-group editor.

Practical approach:

1. Create a WordPress user for the client, email as username, strong password.
2. Use **Members** or **User Role Editor** (or a tiny `add_role()` in the theme) to clone Editor and strip `switch_themes`, `edit_themes`, `install_plugins`, `activate_plugins`, `update_core`, `manage_options`, `edit_theme_options`.
3. If ACF field groups should be developer-only, do not grant `manage_options` and consider [ACF’s field group visibility](https://www.advancedcustomfields.com/resources/how-to-hide-acf-menu-from-clients/) so clients never see **ACF** in the menu.
4. Log in as that user in a private window. Confirm they see **Events / Sermons / Pages** with the ACF metaboxes, and they cannot upload a new theme.

Example (theme or small mu-plugin). Adjust capabilities to the CPTs you registered:

```php
add_action('init', function () {
    if (get_role('content_editor')) {
        return;
    }
    add_role('content_editor', 'Content editor', [
        'read'         => true,
        'upload_files' => true,
        'edit_posts'   => true,
        'publish_posts'=> true,
        'edit_pages'   => true,
        'publish_pages'=> true,
    ]);
});
```

If you renamed CPT capabilities in ACF (**Permissions → Rename Capabilities**), grant `edit_events`, `publish_events`, etc. explicitly.

When the site is live: **disable NovaMira AI abilities** on production. Clients should never have that plugin “on” with a connected agent.

---

## Prompt pack (paste into the connected agent)

Use after NovaMira + ACF are installed and abilities are on.

**Inventory**

> Read this WordPress site: active theme, plugins, templates, and any ACF field groups. Summarize in a table. Do not write files yet.

**Convert**

> Turn the HTML in [path] into the active theme. Preserve CSS class names. header.php / footer.php / front-page.php first. Enqueue existing assets. Stop and show a file list before other templates.

**Fields**

> For front-page.php, list every hardcoded string and image. Propose an ACF field group “Homepage” with field names, types, and location = Front Page. Create the group, then replace the hardcoded values with get_field() / wp_get_attachment_image().

**CPTs**

> Register Event, Sermon, and Speaker with ACF. Add field groups. Create archive and single templates that reuse the HTML card markup.

**Lock down**

> List admin menu items the content_editor role can still see that they should not. Do not change production. Propose the capability removals only.

---

## Official docs

- NovaMira: [install](https://novamira.ai/docs/getting-started/installation/), [configure](https://novamira.ai/docs/getting-started/configuration/), [connect](https://novamira.ai/docs/getting-started/connecting/), [recommended setup](https://novamira.ai/docs/security/recommended-setup/), [ACF (Pro)](https://novamira.ai/mcp/acf/)
- ACF: [getting started](https://www.advancedcustomfields.com/resources/getting-started-with-acf/), [field groups](https://www.advancedcustomfields.com/resources/creating-a-field-group/), [get_field](https://www.advancedcustomfields.com/resources/get_field/), [post types](https://www.advancedcustomfields.com/resources/registering-a-custom-post-type/)
- WordPress: [style.css](https://developer.wordpress.org/themes/basics/main-stylesheet-style-css/), [template files](https://developer.wordpress.org/themes/basics/template-files/), [roles](https://wordpress.org/documentation/article/roles-and-capabilities/)

Video description (chapters and links) is saved next to this file as `video-description.txt`.
