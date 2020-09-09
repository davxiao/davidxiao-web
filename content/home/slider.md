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
height = "400px"

# Slides.
# Duplicate an `[[item]]` block to add more slides.

[[item]]
  title = "Build a Static Blog Site in 2020"
  content = "Wait... What does it mean by *\"static\"?* Can visitors post comments at all? Let's take a closer look 😀"
  align = "left"  # Choose `center`, `left`, or `right`.

  overlay_color = "#555"  # An HTML color value.
  overlay_img = "headers/athousandsitesinone-color.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.85  # Darken the image. Value in range 0-1.

  cta_label = "Check it Out"
  cta_url = "/category/site-building/"
  cta_icon_pack = "fas"
  cta_icon = "sitemap"

[[item]]
  title = "My Homelab"
  content = "I *DIYed* my homelab server with [Proxmox](https://www.proxmox.com/en/proxmox-ve) and a few open source tools. You can do it too 🙌 🎉 🍸"
  align = "left"  # Choose `center`, `left`, or `right`.

  # Overlay a color or image (optional).
  #   Deactivate an option by commenting out the line, prefixing it with `#`.
  overlay_color = "#666"  # An HTML color value.
  overlay_img = "headers/wood-texture-1066656.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.7  # Darken the image. Value in range 0-1.

  # Call to action button (optional).
  #   Activate the button by specifying a URL and button label below.
  #   Deactivate by commenting out parameters, prefixing lines with `#`.
  cta_label = "Check it Out"
  cta_url = "/category/homelab/"
  cta_icon_pack = "fas"
  cta_icon = "server"

[[item]]
  title = "What I Have Been Reading"
  content = "My reading list of nonfiction books. I enjoy reading them but you can draw your own conclusions."
  align = "left"

  overlay_color = "#333"  # An HTML color value.
  overlay_img = "headers/book-unsplash.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.85  # Darken the image. Value in range 0-1.

  cta_label = "Show Me the List"
  cta_url = "/post/what-ive-been-reading/"
  cta_icon_pack = "fas"
  cta_icon = "book-reader"

+++
