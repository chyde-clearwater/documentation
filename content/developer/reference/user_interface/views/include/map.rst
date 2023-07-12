.. _reference/user_interface/views/map:

Map
===

.. raw:: html

   <span class="badge" style="background-color:#AD5E99">Enterprise feature</span>

This view is able to display records on a map and the routes between them. The records are represented by pins. It also allows the visualization of fields from the model in a popup tied to the record's pin.

.. note::

    The model on which the view is applied should contain a `res.partner` many2one since the view relies on the `res.partner`'s address and coordinates fields to localize the records.

API
---

The view uses location data platforms' API to fetch the tiles (the map's background), do the geoforwarding (converting addresses to a set of coordinates) and fetch the routes.
The view implements two API, OpenStreetMap and MapBox. OpenStreetMap is used by default and is able to fetch `tiles`_ and do `geoforwarding`_. This API does not require a token.
As soon as a valid `MapBox`_ token is provided in the general settings the view switches to the MapBox API. This API is faster and allows the computation of routes. A token can be obtained by `signing up`_ to MapBox.

Structural components
---------------------

The view's root element is ``<map>``. It can have the following attributes:


.. rst-class:: o-definition-list

``res_partner``
    Contains the `res.partner` many2one. If not provided the view resorts to create an empty map.
``default_order``
    If a field is provided the view overrides the model's default order. The field must be part of the model on which the view is applied, not from `res.partner`.
``routing``
    if ``1`` display the routes between the records. The view needs a valid MapBox token and at least two located records (i.e the records have a `res.partner` many2one and the partner has an address or valid coordinates).
``hide_name``
    if ``1`` hide the name from the pin's popup (default: ``0``).
``hide_address``
    if ``1`` hide the address from the pin's popup (default: ``0``).
``hide_title``
    if ``1`` hide the title from the pin list (default: ``0``).
``panel_title``
    String to display as title of the pin list. If not provided, the title is the action's name or "Items" if the view is not in an action.
``limit``
    Maximum number of records to fetch (default: ``80``). It must be a positive integer.

The ``<map>`` element can contain multiple ``<field>`` elements. Each ``<field>`` element is interpreted as a line in the pin's popup. The field's attributes are the following:

.. rst-class:: o-definition-list

``name``
    The field to display.
``string``
    String to display before the field's content. It can be used as a description.

For example here is a map:
    .. code-block:: xml

        <map res_partner="partner_id" default_order="date_begin" routing="1" hide_name="1">
            <field name="partner_id" string="Customer Name"/>
        </map>


.. _geoforwarding: https://nominatim.org/release-docs/develop/
.. _MapBox: https://docs.mapbox.com/api/
.. _signing up: https://account.mapbox.com/auth/signup/
.. _tiles: https://wiki.openstreetmap.org/wiki/Tile_data_server
