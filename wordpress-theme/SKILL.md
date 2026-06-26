---
name: wordpress-theme
description: >
  Use this skill whenever the user wants to create, build, modify, or generate a WordPress theme
  or any WordPress theme file. Triggers include: "WordPress theme", "WP theme", "theme.json",
  "functions.php", "template hierarchy", "block theme", "classic theme", "FSE", "Full Site Editing",
  "child theme", "page template", "custom post type", "WooCommerce theme", "Gutenberg blocks",
  "wp_enqueue", "the Loop", "template parts", or any request to generate PHP/CSS files destined
  for a WordPress installation. Also trigger when the user asks to style or design a WordPress
  site, build a landing page in WordPress, or create theme files for a client project. Always
  read this skill before writing any WordPress theme code — even for single files like style.css
  or functions.php — because WordPress has strict structural requirements that differ from plain HTML/CSS.
---

# WordPress Theme Development

You are a senior WordPress theme developer. Every theme you produce must be structurally correct,
performant, accessible, and Gutenberg-compatible. Always ask or infer whether the user wants a
**Classic Theme** or a **Block (FSE) Theme** before writing code — the file structures differ
significantly. When in doubt, default to a **Block Theme** as it is the modern WordPress standard
(WP 5.9+).

---

## Step 1 — Clarify Before Writing

Before generating any files, confirm:
1. **Theme type**: Block (FSE) or Classic?
2. **Child theme?**: Is this extending an existing theme (Astra, GeneratePress, Kadence, etc.)?
3. **Purpose**: Blog, business, portfolio, WooCommerce store, landing page?
4. **Design direction**: Reference the `frontend-design` skill for palette, typography, and layout.
5. **Required features**: Custom post types, ACF fields, WooCommerce, multilingual (WPML/Polylang)?

Read `/mnt/skills/public/frontend-design/SKILL.md` before making any visual/design decisions.

---

## Theme Type Decision Tree

```
Does the user want to use the WP Site Editor (full visual control)?
  YES → Block Theme (FSE) with theme.json
  NO  → Classic Theme with PHP templates

Is this extending an existing theme?
  YES → Child Theme (either type)
  NO  → Standalone theme
```

---

## Block Theme (FSE) — File Structure

```
theme-name/
├── style.css                  ← Required: theme header
├── theme.json                 ← Design tokens, settings, styles
├── functions.php              ← Enqueues, supports, block patterns
├── index.php                  ← Fallback (can be empty)
├── templates/
│   ├── index.html             ← Default template
│   ├── single.html            ← Single post
│   ├── page.html              ← Static page
│   ├── archive.html           ← Archive/category
│   ├── 404.html               ← Not found
│   └── front-page.html        ← Homepage (optional)
├── parts/
│   ├── header.html            ← Site header block template part
│   ├── footer.html            ← Site footer block template part
│   └── sidebar.html           ← Optional sidebar
├── patterns/
│   └── hero-section.php       ← Reusable block patterns
└── assets/
    ├── css/
    ├── js/
    └── images/
```

### style.css Required Header (Block Theme)
```css
/*
Theme Name: Theme Name Here
Theme URI: https://md3digitalsolutions.com
Author: MD3 Digital Solutions Inc
Author URI: https://md3digitalsolutions.com
Description: Theme description here.
Version: 1.0.0
Requires at least: 6.0
Tested up to: 6.7
Requires PHP: 8.0
License: GNU General Public License v2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html
Text Domain: theme-slug
Tags: block-patterns, full-site-editing, custom-colors, custom-fonts
*/
```

### theme.json — Design Tokens
```json
{
  "$schema": "https://schemas.wp.org/trunk/theme.json",
  "version": 3,
  "settings": {
    "color": {
      "palette": [
        { "slug": "primary", "color": "#YOUR_COLOR", "name": "Primary" },
        { "slug": "secondary", "color": "#YOUR_COLOR", "name": "Secondary" },
        { "slug": "accent", "color": "#YOUR_COLOR", "name": "Accent" },
        { "slug": "base", "color": "#ffffff", "name": "Base" },
        { "slug": "contrast", "color": "#111111", "name": "Contrast" }
      ],
      "custom": false,
      "customGradient": false
    },
    "typography": {
      "fontFamilies": [
        {
          "fontFamily": "\"Font Name\", sans-serif",
          "slug": "primary",
          "name": "Primary Font"
        }
      ],
      "fontSizes": [
        { "slug": "small", "size": "0.875rem", "name": "Small" },
        { "slug": "medium", "size": "1rem", "name": "Medium" },
        { "slug": "large", "size": "1.5rem", "name": "Large" },
        { "slug": "x-large", "size": "2rem", "name": "X Large" },
        { "slug": "xx-large", "size": "3rem", "name": "XX Large" }
      ]
    },
    "spacing": {
      "spacingScale": { "steps": 7 },
      "units": ["px", "%", "em", "rem", "vw"]
    },
    "layout": {
      "contentSize": "800px",
      "wideSize": "1200px"
    }
  },
  "styles": {
    "color": {
      "background": "var(--wp--preset--color--base)",
      "text": "var(--wp--preset--color--contrast)"
    },
    "typography": {
      "fontFamily": "var(--wp--preset--font-family--primary)",
      "fontSize": "var(--wp--preset--font-size--medium)",
      "lineHeight": "1.6"
    },
    "elements": {
      "link": {
        "color": { "text": "var(--wp--preset--color--primary)" },
        ":hover": { "color": { "text": "var(--wp--preset--color--accent)" } }
      },
      "h1": { "typography": { "fontSize": "var(--wp--preset--font-size--xx-large)", "fontWeight": "700" } },
      "h2": { "typography": { "fontSize": "var(--wp--preset--font-size--x-large)", "fontWeight": "600" } },
      "h3": { "typography": { "fontSize": "var(--wp--preset--font-size--large)", "fontWeight": "600" } }
    }
  }
}
```

---

## Classic Theme — File Structure

```
theme-name/
├── style.css                  ← Required: theme header + base styles
├── functions.php              ← All theme setup, hooks, enqueues
├── index.php                  ← Main fallback template
├── header.php                 ← get_header() target
├── footer.php                 ← get_footer() target
├── sidebar.php                ← get_sidebar() target
├── single.php                 ← Single post template
├── page.php                   ← Static page template
├── archive.php                ← Archive/taxonomy template
├── category.php               ← Category archive (optional)
├── 404.php                    ← Not found page
├── search.php                 ← Search results
├── searchform.php             ← Custom search form
├── comments.php               ← Comments template
├── screenshot.png             ← 1200×900px theme screenshot
├── template-parts/
│   ├── content.php            ← Post content partial
│   ├── content-single.php     ← Single post content partial
│   └── content-none.php       ← No results partial
└── assets/
    ├── css/
    │   └── main.css
    ├── js/
    │   └── main.js
    └── images/
```

### functions.php — Required Setup
```php
<?php
if ( ! defined( 'ABSPATH' ) ) exit;

function theme_slug_setup() {
    // Theme supports
    add_theme_support( 'title-tag' );
    add_theme_support( 'post-thumbnails' );
    add_theme_support( 'automatic-feed-links' );
    add_theme_support( 'html5', [ 'search-form', 'comment-form', 'comment-list', 'gallery', 'caption', 'style', 'script' ] );
    add_theme_support( 'align-wide' );
    add_theme_support( 'editor-styles' );
    add_theme_support( 'responsive-embeds' );
    add_theme_support( 'wp-block-styles' );

    // Menus
    register_nav_menus( [
        'primary' => __( 'Primary Menu', 'theme-slug' ),
        'footer'  => __( 'Footer Menu', 'theme-slug' ),
    ] );

    // Image sizes
    add_image_size( 'theme-featured', 1200, 630, true );
}
add_action( 'after_setup_theme', 'theme_slug_setup' );

// Enqueue scripts and styles
function theme_slug_enqueue() {
    wp_enqueue_style( 'theme-slug-style', get_stylesheet_uri(), [], wp_get_theme()->get('Version') );
    wp_enqueue_style( 'theme-slug-main', get_template_directory_uri() . '/assets/css/main.css', [], wp_get_theme()->get('Version') );
    wp_enqueue_script( 'theme-slug-main', get_template_directory_uri() . '/assets/js/main.js', [], wp_get_theme()->get('Version'), true );
}
add_action( 'wp_enqueue_scripts', 'theme_slug_enqueue' );

// Widget areas
function theme_slug_widgets_init() {
    register_sidebar( [
        'name'          => __( 'Primary Sidebar', 'theme-slug' ),
        'id'            => 'sidebar-1',
        'before_widget' => '<section id="%1$s" class="widget %2$s">',
        'after_widget'  => '</section>',
        'before_title'  => '<h2 class="widget-title">',
        'after_title'   => '</h2>',
    ] );
}
add_action( 'widgets_init', 'theme_slug_widgets_init' );
```

### The Loop — Standard Pattern
```php
<?php if ( have_posts() ) : ?>
    <?php while ( have_posts() ) : the_post(); ?>
        <article id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
            <header class="entry-header">
                <?php the_title( '<h2 class="entry-title"><a href="' . esc_url( get_permalink() ) . '">', '</a></h2>' ); ?>
                <div class="entry-meta">
                    <time datetime="<?php echo esc_attr( get_the_date( 'c' ) ); ?>"><?php the_date(); ?></time>
                    <?php the_author_posts_link(); ?>
                </div>
            </header>
            <?php if ( has_post_thumbnail() ) : ?>
                <div class="post-thumbnail">
                    <a href="<?php the_permalink(); ?>"><?php the_post_thumbnail( 'theme-featured' ); ?></a>
                </div>
            <?php endif; ?>
            <div class="entry-content">
                <?php the_excerpt(); ?>
            </div>
        </article>
    <?php endwhile; ?>
    <?php the_posts_pagination(); ?>
<?php else : ?>
    <?php get_template_part( 'template-parts/content', 'none' ); ?>
<?php endif; ?>
```

---

## Child Theme

Use when extending Astra, GeneratePress, Kadence, Divi, or any parent theme.

```
child-theme-name/
├── style.css       ← Declares parent via Template header
└── functions.php   ← Enqueues parent + child styles
```

```css
/*
Theme Name: Parent Theme Child
Template: parent-theme-folder-name
Version: 1.0.0
Text Domain: parent-theme-child
*/
```

```php
<?php
function child_theme_enqueue() {
    wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );
    wp_enqueue_style( 'child-style', get_stylesheet_uri(), [ 'parent-style' ], wp_get_theme()->get('Version') );
}
add_action( 'wp_enqueue_scripts', 'child_theme_enqueue' );
```

---

## WooCommerce Support

Add to `functions.php` to declare WooCommerce compatibility:
```php
function theme_slug_woocommerce_setup() {
    add_theme_support( 'woocommerce', [
        'thumbnail_image_width' => 450,
        'single_image_width'    => 600,
        'product_grid'          => [ 'default_rows' => 3, 'min_rows' => 1, 'default_columns' => 3, 'min_columns' => 1, 'max_columns' => 6 ],
    ] );
    add_theme_support( 'wc-product-gallery-zoom' );
    add_theme_support( 'wc-product-gallery-lightbox' );
    add_theme_support( 'wc-product-gallery-slider' );
}
add_action( 'after_setup_theme', 'theme_slug_woocommerce_setup' );
```

For WooCommerce template overrides, copy from:
`wp-content/plugins/woocommerce/templates/` → `wp-content/themes/your-theme/woocommerce/`

---

## Custom Post Types (CPT)

```php
function theme_slug_register_cpts() {
    register_post_type( 'portfolio', [
        'labels'      => [ 'name' => 'Portfolio', 'singular_name' => 'Project' ],
        'public'      => true,
        'has_archive' => true,
        'supports'    => [ 'title', 'editor', 'thumbnail', 'excerpt' ],
        'menu_icon'   => 'dashicons-portfolio',
        'rewrite'     => [ 'slug' => 'portfolio' ],
        'show_in_rest'=> true,  // Required for Gutenberg support
    ] );
}
add_action( 'init', 'theme_slug_register_cpts' );
```

---

## Performance & Security Checklist

Always apply these to every theme:

- [ ] `wp_head()` and `wp_footer()` present in header/footer templates
- [ ] All output escaped: `esc_html()`, `esc_url()`, `esc_attr()`, `wp_kses_post()`
- [ ] Nonces used for any form submissions
- [ ] `ABSPATH` check at top of all PHP files
- [ ] Scripts loaded in footer where possible (`true` as last arg)
- [ ] Images use `loading="lazy"` attribute
- [ ] CSS variables used instead of hardcoded values
- [ ] No inline styles unless dynamically generated
- [ ] Text domain matches theme slug for all `__()` / `_e()` calls
- [ ] `screenshot.png` is exactly 1200×900px

---

## Template Hierarchy (Classic)

WordPress loads templates in this order (first match wins):

```
Single Post:    single-{post-type}-{slug}.php → single-{post-type}.php → single.php → index.php
Page:           {page-slug}.php → page-{id}.php → page.php → index.php
Category:       category-{slug}.php → category-{id}.php → category.php → archive.php → index.php
Custom Archive: archive-{post-type}.php → archive.php → index.php
Front Page:     front-page.php → home.php → index.php
```

---

## MD3 Digital Solutions — Theme Branding Defaults

When generating themes for MD3 Digital Solutions clients, include in comments or README:
- **Agency**: MD3 Digital Solutions Inc
- **Contact**: darielcorchado@gmail.com | 786-757-4345
- **Author URI**: Reference in style.css Author URI field

---

## References

For extended documentation, read these when needed:
- `references/block-patterns.md` — Block pattern registration and examples
- `references/acf-integration.md` — Advanced Custom Fields in templates
- `references/gutenberg-blocks.md` — Custom block registration with `block.json`
