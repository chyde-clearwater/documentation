.. _reference/user_interface/views/search:

Search
======

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <search>
      ...
    </search>

  .. image:: views/search.svg
    :class: col-xxl-6

Search views are different from other view types: they don't display
*content*. Although they apply to a specific model, they are used to filter
another view's content (generally aggregated views
e.g. :ref:`reference/user_interface/views/list` or :ref:`reference/user_interface/views/graph`).

The root element of search views is ``<search>``. It takes no attributes.

Possible children elements of the list view are: ``field``, ``filter``, ``separator``,
``group`` or ``searchpanel``

.. _reference/user_interface/views/search/field:

<field>: field usable as filter
-------------------------------

.. code-block:: xml

  <search>
    <field name="FIELD_NAME"/>
  </search>

fields define domains or contexts with user-provided values. When search
domains are generated, field domains are joined with each other and
with filters using the **AND** operator.

Fields can have the following attributes:

:name:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name (mandatory)

  the name of the field to filter on

:string:
  string_ (optional)

  the field's label

:operator:
  string_ (default: ``=``)

  by default, fields generate domains of the form :samp:`[({name},
  {operator}, {provided_value})]` where ``name`` is the field's name and
  ``provided_value`` is the value provided by the user, possibly
  filtered or transformed (e.g. a user is expected to provide the
  *label* of a selection field's value, not the value itself).

  The ``operator`` attribute allows overriding the default operator,
  which depends on the field's type (e.g. ``=`` for float fields but
  ``ilike`` for char fields or ``child_of`` for many2one)

:filter_domain:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a :ref:`reference/orm/domains` (default: ``False``)

  complete domain to use as the field's search domain, can use a
  ``self`` variable to inject the provided value in the custom
  domain. Can be used to generate significantly more flexible domains
  than ``operator`` alone (e.g. searches on multiple fields at once)

  If both ``operator`` and ``filter_domain`` are provided,
  ``filter_domain`` takes precedence.

:context:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a dict_ (default: ``{}``)

  allows adding context keys, including the user-provided values (which
  as for ``domain`` are available as a ``self`` variable, an array of
  values e.g. ``[id_1, id_2]`` for a :class:`~odoo.fields.Many2one` field).
  By default, fields don't generate domains.

  .. note:: the domain and context are inclusive and both are generated
            if a ``context`` is specified. To only generate context
            values, set ``filter_domain`` to an empty list:
            ``filter_domain="[]"``

:domain:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a :ref:`reference/orm/domains` (default: ``False``)

  if the field can provide an auto-completion
  (e.g. :class:`~odoo.fields.Many2one`), filters the possible
  completion results.

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  make the field only available to specific users

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  standard dynamic attributes based on record values. Hide the field
  if truthy or if the evaluate expression is truthy.

  .. code-block:: xml

    <field name="fname_a" invisible="True"/>
    <field name="fname_b" invisible="fname_c != 3 and fname_a == parent.fname_d"/>

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <search>
      <field name="name" string="My Custom Name"/>
      <field name="amount"/>
      <field name="company_id" invisible="1"/>
      <field name="currency_id"/>
      <field name="ref" filter_domain="[('name', 'like', self)]"/>
    </search>

  .. image:: views/search_field.svg
    :class: col-xxl-6

.. _reference/user_interface/views/search/filter:

<filter>: predefined Filters
----------------------------

.. code-block:: xml

  <search>
    <filter string="LABEL" domain="DOMAIN"/>
  </search>

a filter is a predefined toggle in the search view, it can only be enabled
or disabled. Its main purposes are to add data to the search context (the
context passed to the data view for searching/filtering), or to append new
sections to the search filter.

Filters can have the following attributes:

:string:
  string_ (mandatory)

  the label of the filter

:domain:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a :ref:`reference/orm/domains` (default: ``False``)

  will be appended to the action's domain as part of the search domain.

:date:
  string_ (optional) :ref:`model <reference/orm/model>` field name

  the name of a field of type ``date`` or ``datetime``.
  Using this attribute has the effect to create
  a set of filters available in a submenu
  of the filters menu. The filters proposed are time dependent
  but not dynamic in the sense that their domains are evaluated
  at the time of the control panel instantiation.

  Example:

  .. code-block:: xml

    <filter name="filter_create_date" date="create_date" string="Creation Date"/>

  The example above allows to easily search for records with creation date field
  values in one of the periods below (if the current month is August 2019).

  .. code-block:: text

    Create Date >
      August
      July
      June
      Q4
      Q3
      Q2
      Q1
    --------------
      2019
      2018
      2017

  Multi selection of options is allowed.

:default_period:
  string_ (optional)  chooses from ``today``, ``this_week``, ``this_month``, ``last_month``,
  ``antepenultimate_month``, ``fourth_quarter``, ``third_quarter``, ``second_quarter``,
  ``first_quarter``, ``this_year``, ``last_year`` or ``antepenultimate_year``

  only makes sense for a filter with non empty ``date`` attribute.
  determines which periods are activated if the filter is in the
  default set of filters activated at the view initialization. If not provided,
  'this_month' is used by default.

  The attribute accepts comma separated values.

  Examples:

  .. code-block:: xml

    <filter name="filter_create_date" date="create_date" string="Creation Date" default_period="this_week"/>
    <filter name="filter_create_date" date="create_date" string="Creation Date" default_period="this_year,last_year"/>

:context:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a dict_ (default: ``{}``)

  a Python dictionary, merged into the action's domain to generate the
  search domain

  The key ``group_by`` can be used to define a groupby available in the
  'Group By' menu. The 'group_by' value can be a valid field name.

  .. code-block:: xml

      <filter name="groupby_category" string="Category" context="{'group_by': 'category_id'}"/>

  The groupby defined above allows to group data by category.

  When the field is of type ``date`` or ``datetime``, the filter generates a submenu of the Group By
  menu in which the following interval options are available: day, week, month, quarter, year.

  In case the filter is in the default set of filters activated at the view initialization,
  the records are grouped by month by default. This can be changed by using the syntax
  'date_field:interval' as in the following example.

  Example:

  .. code-block:: xml

      <filter name="groupby_create_date" string="Creation Date" context="{'group_by': 'create_date:week'}"/>

  .. note::
      The results of read_groups grouped on a field may be influenced by its group_expand attribute,
      allowing to display empty groups when needed.  For more information, please refer to
      :class:`~odoo.fields.Field` attributes documentation.

:name:
  string_ (optional)

  logical name for the filter, can be used to :ref:`enable it by default
  <reference/user_interface/views/search/defaults>`, can also be used as
  :ref:`inheritance hook <reference/view_record/inheritance>`

:help:
  string_ (optional)

  a longer explanatory text for the filter, may be displayed as a
  tooltip

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  make the field only available to specific users

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  standard dynamic attributes based on record values. Hide the field
  if truthy or if the evaluate expression is truthy.

  .. code-block:: xml

    <field name="fname_a" invisible="True"/>
    <field name="fname_b" invisible="fname_c != 3 and fname_a == parent.fname_d"/>

.. note::

  Sequences of filters (without non-filters separating them) are treated
  as inclusively composited: they will be composed with ``OR`` rather
  than the usual ``AND``, e.g.

  .. code-block:: xml

    <filter domain="[('state', '=', 'draft')]"/>
    <filter domain="[('state', '=', 'done')]"/>

  if both filters are selected, will select the records whose ``state``
  is ``draft`` or ``done``, but

  .. code-block:: xml

    <filter domain="[('state', '=', 'draft')]"/>
    <separator/>
    <filter domain="[('delay', '<', 15)]"/>

  if both filters are selected, will select the records whose ``state``
  is ``draft`` **and** ``delay`` is below 15.

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <search>
      <filter string="My Custom Name"
          domain="[('name', 'ilike', 'AAA')]"/>
      <filter string="My orders"
          domain="[('user_id', '=', uid)]"/>
      <filter string="Category"
          context="{'group_by': 'category_id'}"/>
    </search>

  .. image:: views/search_filter.svg
    :class: col-xxl-6

.. _reference/user_interface/views/search/separator:

<separator>: separate groups of filters
---------------------------------------

.. code-block:: xml

  <search>
    <FILTERS/>
    <separator/>
    <FILTERS/>
  </search>

can be used to separates groups of :ref:`filters <reference/user_interface/views/search/filter>` in simple search views.
`group` is more readable.

.. user_interface/views/search/group:

<group>: separate groups of filters
-----------------------------------

.. code-block:: xml

  <search>
    <group expand="0" string="LABEL">
      <FILTERS/>
    </group>
  </search>

can be used to separate groups of :ref:`filters <reference/user_interface/views/search/filter>`, more readable than
``separator`` in complex search views

<searchpanel>: display a search panel
-------------------------------------

.. code-block:: xml

  <search>
    <searchpanel>
      <FIELDS/>
    </searchpanel>
  </search>

allows to display a search panel on the left of any multi records view.

This tool allows to quickly filter data on the basis of given fields. The fields
are specified as direct children of the ``searchpanel`` with tag name ``field``.

.. rst-class:: o-definition-list

``<field>``
  Fields in searchpanel can have the following attributes:

  :name:
    string_ (mandatory) :ref:`model <reference/orm/model>` field name

    the name of the field to filter on

  :select:
    string_ chooses from ``one``, ``multi``, ``groups``, ``string``, ``icon`` or ``color`` (default: ``one``)

    determines the behavior and display.

    .. rst-class:: o-definition-list

    ``one`` (default)
      at most one value can be selected. Supported field types are
      many2one and selection.

    ``multi``
      several values can be selected (checkboxes). Supported field
      types are many2one, many2many and selection.

    ``groups``
      restricts to specific users

    ``string``
      determines the label to display

    ``icon``
      specifies which icon is used

    ``color``
      determines the icon color

    Additional optional attributes are available in the ``multi`` case:

    .. rst-class:: o-definition-list

    ``enable_counters``
      default is false. If set to true the record counters will be computed and
      displayed if non-zero.

      This feature has been implemented in case performances would be too bad.

      Another way to solve performance issues is to properly override the
      ``search_panel_select_range`` and ``search_panel_select_multi_range`` methods.

    ``expand``
      default is false. If set to false categories or filters with 0 records will be hidden.

    ``limit``
      default is 200. Integer determining the maximal number of values to fetch for the field.
      If the limit is reached, no values will be displayed in the search panel and an error message will
      appear instead because we consider that is useless / bad performance-wise. All values will be
      fetched if set to 0.

    Additional optional attributes are available according to the chosen case:

    - For the ``one`` case:

      .. rst-class:: o-definition-list

      ``hierarchize``
        (only available for many2one fields) default is true. Handles the display style of categories :

        If set to true child categories will appear under their related parent.
        If not, all categories will be displayed on the same level.

    - For the ``multi`` case:

      .. rst-class:: o-definition-list

      ``domain``:
        determines conditions that the comodel records have to satisfy.

    A domain might be used to express a dependency on another field (with select="one")
    of the search panel. Consider
    /!\ This attribute is incompatible with a select="one" with enabled counters; if a select="multi"
    has a `domain` attribute, all select="one" will have their counters disabled.

    .. code-block:: xml

      <searchpanel>
        <field name="department_id"/>
        <field name="manager_id" select="multi" domain="[('department_id', '=', department_id)]"/>
      </searchpanel>

    In the above example, the range of values for manager_id (manager names) available at screen
    will depend on the value currently selected for the field ``department_id``.

  :groupby:
    string_ (optional) :ref:`model <reference/orm/model>` field name

    field name of the comodel (only available for many2one and many2many fields). Values will be grouped by that field.

.. _reference/user_interface/views/search/defaults:

Search defaults
---------------

Search fields and filters can be configured through the action's ``context``
using :samp:`search_default_{name}` keys. For fields, the value should be the
value to set in the field, for filters it's a boolean value or a number. For instance,
assuming ``foo`` is a field and ``bar`` is a filter an action context of:

.. code-block:: python

  {
    'search_default_foo': 'acro',
    'search_default_bar': 1
  }

will automatically enable the ``bar`` filter and search the ``foo`` field for
*acro*.

A numeric value (between 1 and 99) can be used to describe the order of default groupbys.
For instance if ``foo`` and ``bar`` refer to two groupbys

.. code-block:: python

  {
    'search_default_foo': 2,
    'search_default_bar': 1
  }

has the effect to activate first ``bar`` then ``foo``.


.. _attributes: https://en.wikipedia.org/wiki/HTML_attribute
.. _bootstrap contextual color: https://getbootstrap.com/docs/3.3/components/#available-variations
.. _`Comma-separated values`: https://en.wikipedia.org/wiki/Comma-separated_values
.. _integer: https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _dict: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
