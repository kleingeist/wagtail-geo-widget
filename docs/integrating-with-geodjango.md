# Integrating with GeoDjango

First make sure you have [GeoDjango](https://docs.djangoproject.com/en/1.10/ref/contrib/gis/) correctly setup and a `PointField` field defined in your model, then add a `GoogleMapsPanel` among your `content_panels`.

```python
from django.contrib.gis.db import models
from wagtailgeowidget.edit_handlers import GoogleMapsPanel


class MyPage(Page):
    location = models.PointField(srid=4326, null=True, blank=True)

    content_panels = Page.content_panels + [
        GoogleMapsPanel('location'),
    ]
```

If you instead want to use Leaflet, just change `GoogleMapsPanel` to `LeafletPanel`


### With an address field

The panel accepts an `address_field` if you want to use the map in coordination with a geo-lookup.

```python
from django.contrib.gis.db import models
from django.utils.translation import gettext as _
from wagtail.admin.edit_handlers import FieldPanel, MultiFieldPanel
from wagtailgeowidget import geocoders
from wagtailgeowidget.edit_handlers import GeoAddressPanel, GoogleMapsPanel


class MyPageWithAddressField(Page):
    address = models.CharField(max_length=250, blank=True, null=True)
    location = models.PointField(srid=4326, null=True, blank=True)

    content_panels = Page.content_panels + [
        MultiFieldPanel([
            GeoAddressPanel("address", geocoder=geocoders.GOOGLE_MAPS),
            GoogleMapsPanel('location', address_field='address'),
        ], _('Geo details')),
    ]
```


### With an zoom field

The panel accepts an `zoom_field` if you want to persist the zoom state.

```python
from django.contrib.gis.db import models
from django.utils.translation import gettext as _
from wagtail.admin.edit_handlers import FieldPanel, MultiFieldPanel
from wagtailgeowidget.edit_handlers import GoogleMapsPanel


class MyPageWithZoomField(Page):
    zoom = models.SmallIntegerField(blank=True, null=True)
    location = models.PointField(srid=4326, null=True, blank=True)

    content_panels = Page.content_panels + [
        MultiFieldPanel([
            FieldPanel('zoom'),
            GoogleMapsPanel('location', zoom_field='zoom'),
        ], _('Geo details')),
    ]
```


### More examples

For more examples, look at the [example](https://github.com/Frojd/wagtail-geo-widget/blob/develop/example/geopage/models.py).
