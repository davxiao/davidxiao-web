+++
# Slider widget.
widget = "slider"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 5  # Order that this section will appear.

# Slide interval.
# Use `false` to disable animation or enter a time in ms, e.g. `5000` (5s).
interval = 7000

# Slide height (optional).
# E.g. `500px` for 500 pixels or `calc(100vh - 70px)` for full screen.
# 70px is the navbar height.
# height = "calc(100vh - 70px)"
height = "300px"

# Slides.
# Duplicate an `[[item]]` block to add more slides.

[[item]]
  title = ""
  content = "Wait... What does it mean by *static*? Can visitors post comments at all? Let's take a closer look 😀"
  align = "center"  # Choose `center`, `left`, or `right`.

  overlay_color = "#404040"  # An HTML color value.
  overlay_img = "headers/volkan-olmez-BVGMRRFQcf8-unsplash.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.45  # Darken the image. Value in range 0-1.

  cta_label = "\"Hugo\" a static web site"
  cta_url = "/category/site-building/"
  cta_icon_pack = "fas"
  cta_icon = "sitemap"

[[item]]
  title = ""
  content = "I *DIYed* my homelab server with Proxmox and a few open source tools. You can do it too 🙌 🎉 🍸"
  align = "center"  # Choose `center`, `left`, or `right`.

  # Overlay a color or image (optional).
  #   Deactivate an option by commenting out the line, prefixing it with `#`.
  overlay_color = "#404040"  # An HTML color value.
  overlay_img = "headers/tim-gouw-GdKxouPYLTU-unsplash.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.45  # Darken the image. Value in range 0-1.

  # Call to action button (optional).
  #   Activate the button by specifying a URL and button label below.
  #   Deactivate by commenting out parameters, prefixing lines with `#`.
  cta_label = "Build a homelab using Proxmox"
  cta_url = "/category/homelab/"
  cta_icon_pack = "fas"
  cta_icon = "server"

[[item]]
  title = ""
  content = "My reading list of nonfiction books. I enjoy reading them but you can draw your own conclusions."
  align = "center"

  overlay_color = "#404040"  # An HTML color value.
  overlay_img = "headers/john-schaidler-9V3Q2W_mRLE-unsplash.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.45

  cta_label = "Show Me the List"
  cta_url = "/post/what-ive-been-reading/"
  cta_icon_pack = "fas"
  cta_icon = "book-reader"

+++
