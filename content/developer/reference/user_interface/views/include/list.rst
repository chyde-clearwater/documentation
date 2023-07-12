.. _reference/user_interface/views/list:

List
====

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      ...
    </tree>

  .. image:: views/list.svg
    :class: col-xxl-6

The root element of list views is ``<tree>``\ [#treehistory]_. The list view's
root can have the following attributes_:

:string:
  string_ (default: ``''``)

  This view title is displayed only if you open an action that has no name and
  whose target is 'new' (opening a dialog)

:create:
  boolean_ (default: ``True``)

  Disable/enable record creation on the view.

:edit:
  boolean_ (default: ``True``)

  Disable/enable record editing on the view.

  If the ``edit`` attribute is set to ``false``, the ``editable`` option will be ignored.

:delete:
  boolean_ (default: ``True``)

  Disable/enable record deletion on the view through the **Action** dropdown.

:import:
  boolean_ (default: ``True``)

  Disable/enable record import data on the view.

:export_xlsx:
  boolean_ (default: ``True``)

  Disable/enable record export data on the view.

:editable:
  string_ (optional) chooses from ``top`` or ``bottom``

  by default, selecting a list view's row opens the corresponding
  :ref:`form view <reference/user_interface/views/form>`. The ``editable`` attributes
  makes the list view itself editable in-place.

  Valid values are ``top`` and ``bottom``, making *new* records appear respectively at
  the top or bottom of the list.

  The architecture for the inline :ref:`form view <reference/user_interface/views/form>`
  is derived from the list view. Most attributes valid on a :ref:`form view
  <reference/user_interface/views/form>`'s fields and buttons are thus accepted by list
  views although they may not have any meaning if the list view is non-editable.

  If the ``edit`` attribute is set to ``false``, the ``editable`` option **will be ignored**.

:multi_edit:
  ``1`` (optional)

  editable or not editable list can activate the multi-editing feature that allows to
  change the same field to the same value for multiple records in a single operation by
  defining the ``multi_edit="1"``

:default_group_by:
  string_ (optional) :ref:`model <reference/orm/model>` field name

  whether the list view should be grouped if no grouping is specified via the action or
  the current search. Should be the name of the field to group by when no grouping is
  otherwise specified

:default_order:
  `Comma-separated values`_ (optional)

  overrides the ordering of the model, replacing the model's order (:attr:`~odoo.models.BaseModel._order` model attribute).
  The value is a comma-separated list of :ref:`fields <reference/orm/fields>`, postfixed by ``desc`` to
  sort in reverse order:

  .. code-block:: xml

    <tree default_order="sequence,name desc">
      ...
    </tree>

:decoration-bf:
:decoration-it:
:decoration-danger:
:decoration-warning:
:decoration-info:
:decoration-muted:
:decoration-primary:
:decoration-success:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  allow changing the style of a cell's text based on the corresponding
  record's attributes:

  * ``bf`` for ``font-weight: bold``
  * ``it`` for ``font-style: italic``
  * ``danger``, ``info``, ``muted``, ``primary``, ``success`` or ``warning`` add relative `bootstrap contextual color`_.

  Define a conditional display of a record in the style of a row's text based
  on the corresponding record's attributes. For each record, the expression is
  evaluated with the record's attributes as context values and if ``true``, the
  corresponding style is applied to the row.

  .. code-block:: xml

    <tree decoration-danger="field_qty > field_limit">
      ...
    </tree>

:limit:
  integer_ (default: ``80`` for list view and ``40`` for x2many list in form view)

  the default size of a page. It must be a positive integer

:groups_limit:
  integer_ (default: ``80`` for list view and ``40`` for x2many list in form view)

  when the list view is grouped, the default number of groups of a page. It must be a
  position integer

:expand:
  boolean_ (default: ``False``)

  when the list view is grouped, automatically open the first level of groups if set
  to true. (Warning: It may be slow depending on the number of groups)

Possible children elements of the list view are: ``button``, ``field``, ``groupby``,
``header`` or ``control``

.. _reference/user_interface/views/list/field:

<field>: render formatted values
--------------------------------

.. code-block:: xml

  <tree>
    <field name="FIELD_NAME"/>
  </tree>

defines a column where the corresponding field should be displayed for
each record. Can use the following attributes:

:name:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name

  the name of the field to display in the current model. A given name
  can only be used once per view

:string:
  string_ (optional)

  the title of the field's column (by default, uses the ``string`` of
  the model's field)

:optional:
  string_ (optional) chooses from ``hide`` or ``show``

  if set, the visibility of the field is optional. The display can be chosen
  via a button in the list view. By default fields will be visible if the value
  is `show` or not visible if `hide`.

  .. code-block:: xml

    <field name="fname_a" optional="hide"/>

:readonly:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  standard dynamic attributes based on record values. If the value is truthy
  or if the evaluate expression is truthy, display the field in both readonly
  and edit mode, but never make it editable.

  .. code-block:: xml

    <field name="fname_a" readonly="True"/>
    <field name="fname_b" readonly="name_a in [fname_b, parent.fname_d]"/>

:required:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  standard dynamic attributes based on record values. If the value is truthy
  or if the evaluate expression is truthy, generates an error and prevents
  saving the record if the field doesn't have a value.

  .. code-block:: xml

    <field name="fname_a" required="True"/>
    <field name="fname_b" required="fname_c != 3 and fname_a == parent.fname_d"/>

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  standard dynamic attributes based on record values. Hide the field if truthy
  or if the evaluate expression is truthy.

  Fetches and stores the field, but doesn't display the column in the
  table. Necessary for fields which shouldn't be displayed but are
  used by e.g. ``@colors`` or an expression.

  .. code-block:: xml

    <field name="fname_a" invisible="True"/>
    <field name="fname_b" invisible="fname_c != 3 and fname_a == parent.fname_d"/>

:column_invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  Fetches and stores the field, but doesn't display the column in the table.
  Necessary for fields which shouldn't be displayed but are used by e.g.
  ``@colors`` or an expression.

  Unlike ``invisible``, if the evaluate expression is truly the entire column
  invisible and is evaluate without the subtree values.

  .. code-block:: xml

    <field name="product_is_late" column_invisible="parent.has_late_products == False"/>

  .. note::
    Only in case of list sub-views (One2many/Many2many display in a form view).

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  lists the groups which should be able to see the field (removed server side
  if the user's groups do not match)

  .. code-block:: xml

    <field name="fname" groups="base.group_no_one,!base.group_multi_company"/>

:decoration-bf:
:decoration-it:
:decoration-danger:
:decoration-warning:
:decoration-info:
:decoration-muted:
:decoration-primary:
:decoration-success:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  allow changing the style of a cell's text based on the corresponding
  record's attributes:

  * ``bf`` for ``font-weight: bold``
  * ``it`` for ``font-style: italic``
  * ``danger``, ``info``, ``muted``, ``primary``, ``success`` or ``warning`` add relative `bootstrap contextual color`_.

  Define a conditional display of a record in the style of a row's text based on the corresponding
  record's attributes.

  Values are Python expressions. For each record, the expression is evaluated
  with the record's attributes as context values and if ``true``, the
  corresponding style is applied to the row. Here are some of the other values
  available in the context:

  * ``uid``: the id of the current user,
  * ``today``: the current local date as a string of the form ``YYYY-MM-DD``,
  * ``now``: same as ``today`` with the addition of the current time.
    This value is formatted as ``YYYY-MM-DD hh:mm:ss``.

  .. code-block:: xml

    <tree decoration-info="state == 'draft'"
          decoration-danger="state == 'help_needed'"
          decoration-bf="state == 'busy'">
      ...
    </tree>

:widget:
  string_ (optional) chooses from ``handle`` or ``progressbar``

  alternate representations for a field's display. Possible list view
  values are (among others):

  .. rst-class:: o-definition-list

  ``handle``
    for ``sequence`` (or ``integer``) fields by which records are
    sorted, instead of displaying the field's value just displays a
    drag&drop icon to reorder records.
  ``progressbar``
    displays ``float`` fields as a progress bar.

  See more information in :ref:`reference/js/widgets`

  .. code-block:: xml

    <tree>
        <field name="sequence" widget="handle"/>
        <field name="level_progress" widget="progressbar"/>
    </tree>

:sum, avg:
  string_ (optional)

  displays the corresponding aggregate at the bottom of the column. The
  aggregation is only computed on *currently displayed* records. The
  aggregation operation must match the corresponding field's
  ``group_operator``

  .. code-block:: xml

    <tree>
      <field name="sent" sum="Total" />
      <field name="clicks_ratio" avg="Average"/>
    </tree>

:width:
  string_/integer_ (for ``editable``) (optional)

  when there is no data in the list, the width of a column can be forced
  by setting this attribute. The value can be an absolute width (e.g.
  '100px'). Note that when there are records in the list, we let the
  browser automatically adapt the column's widths according to their
  content, and this attribute is thus ignored.

:nolabel:
  ``1`` (optional)

  if set to "1", the column header will remain empty. Also, the column
  won't be sortable.

.. note::

  if the list view is ``editable``, any field attribute from the
  :ref:`form view <reference/user_interface/views/form>` is also valid and will
  be used when setting up the inline form view.

.. note:: When a list view is grouped, numeric fields are aggregated and
  displayed for each group. Also, if there are too many records in a group,
  a pager will appear on the right of the group row. For this reason, it is
  not a good practice to have a numeric field in the last column, when the
  list view is in a situation where it can be grouped (it is however fine
  for x2manys field in a form view: they cannot be grouped).

.. _reference/user_interface/views/list/button:

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      <field name="name" string="My Custom Name"/>
      <field name="amount" sum="Total"/>
      <field name="company_id" invisible="1"/>
      <field name="currency_id"/>
      <field name="tax_id"/>
    </tree>

  .. image:: views/list_field.svg
    :class: col-xxl-6

<button>: display button to call action
---------------------------------------

.. code-block:: xml

  <tree>
    <button type="object" name="ACTION" string="LABEL"/>
    <button type="object" name="ACTION" icon="FONT_AWESOME"/>
  </tree>

.. include:: views/include/component_button.rst

:column_invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  same as for :ref:`list field <reference/user_interface/views/list/field>` component.

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      <field name="name"/>
      <button
          type="edit" name="edit"
          icon="fa-edit" title="Edit"/>
      <button
          type="object" name="my_method"
          string="Button1"
          column_invisible="context.get('hide_button')"
          invisible="amount &gt; 3"/>
      <field name="amount"/>
      <field name="currency_id"/>
      <field name="tax_id"/>
    </tree>

  .. image:: views/list_button.svg
    :class: col-xxl-6

.. _reference/user_interface/views/list/groupby:

<groupby>: custom headers when grouping
---------------------------------------

.. code-block:: xml

  <tree>
    ...
    <groupby name="FIELD_NAME">
      <BUTTONS/>
      <FIELDS/>
    </groupby>
  </tree>

defines custom headers (with ``buttons``) for the current view when grouping
records on many2one fields. It is also possible to add ``field``, inside the
``groupby`` which can be used for modifiers. These fields thus belong on the
many2one comodel. These extra fields will be fetched in batch.

:name:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name

  the name of a many2one field (on the current model). Custom header will be
  displayed when grouping the view on this field name.

  A special button (``type="edit"``) can be defined to open the many2one form view.

Below is a possible structure and the representation of its rendering when
group the record by the selected.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      <field name="name"/>
      <field name="amount"/>
      <field name="currency"/>
      <field name="tax_id"/>

      <groupby name="partner_id">
        <button
            type="edit" name="edit"
            icon="fa-edit" title="Edit"/>

        <field name="email"/>
        <button
            type="object" name="my_method"
            string="Button1"
            invisible="email == 'jhon@conor.com'"/>
      </groupby>
    </tree>

  .. image:: views/list_groupby.svg
    :class: col-xxl-6

.. note::

  The ``fields`` inside ``<groupby>`` are only used to fetches and stores the
  value but are never displayed.

.. _reference/user_interface/views/list/header:

<header>: custom buttons in control panel
-----------------------------------------

.. code-block:: xml

  <tree>
    <header>
      <BUTTONS/>
    </header>
    ...
  </tree>

.. include:: views/include/header_buttons.rst

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      <header>
        <button type="object" name="to_draft"
            string="Button1"
            invisible="context.get('hide_button')"/>
      </header>
      <field name="name"/>
      <field name="amount"/>
      <field name="currency"/>
      <field name="tax_id"/>
    </tree>

  .. image:: views/list_header.svg
    :class: col-xxl-6

.. _reference/user_interface/views/list/control:

<control> & <create>: customize "add a line"
--------------------------------------------

.. code-block:: xml

  <tree>
    <control>
      <create string="LABEL"/>
      <BUTTONS/>
    </control>
    ...
  </tree>

defines custom controls for the current view. Does not support any attribute,
but can have children ``<create>``.

``<create>`` adds a button to create a new element on the current list.
It can have the following attributes:

:string:
  string_ (mandatory)

  The text displayed on the button.

:context:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a dict_ (default: ``{}``)

  This context will be merged into the existing context
  when retrieving the default value of the new record.

  For example it can be used to override default values.

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <tree>
      <field name="name"/>
      <field name="amount"/>
      <field name="currency"/>
      <field name="tax_id"/>
      <control>
        <create string="Add a item"/>
        <create string="Add a section"
            context="{'default_type': 'section'}"/>
        <create string="Add a note"
            context="{'default_type': 'note'}"/>
      </control>
    </tree>

  .. image:: views/list_control.svg
    :class: col-xxl-6

.. note:: ``<control>`` makes sense if the parent ``tree`` view is inside a
  :class:`~odoo.fields.One2many` :ref:`relational field <studio/fields/relational-fields>`.

  If any ``<create>`` is defined as children, it will overwrite the default
  "**add a line**" button.

.. [#treehistory] for historical reasons, it has its origin in tree-type views
                later repurposed to a more table/list-type display


.. _attributes: https://en.wikipedia.org/wiki/HTML_attribute
.. _bootstrap contextual color: https://getbootstrap.com/docs/3.3/components/#available-variations
.. _`Comma-separated values`: https://en.wikipedia.org/wiki/Comma-separated_values
.. _integer: https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _dict: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
