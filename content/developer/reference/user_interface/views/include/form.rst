.. _reference/user_interface/views/form:

Form
====

Form views are used to display the data from a single record. Their root element is
``<form>``. They are composed of regular HTML_ with additional structural and semantic
components.

.. code-block:: xml

  <form>
    ...
  </form>

Optional attributes_ are added on root element ``<form>`` to customize the view.

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

:duplicate:
  boolean_ (default: ``True``)

  Disable/enable record duplication on the view through the **Action** dropdown.

:delete:
  boolean_ (default: ``True``)

  Disable/enable record deletion on the view through the **Action** dropdown.

:js_class:
  string_ (optional)

  Name of the javascript component the webclient will instantiating instead of
  the form the view.

:disable_autofocus:
  boolean_ (default: ``False``)

  Disable automatic focussing of the first field in the view.

.. _reference/user_interface/views/form/semantic:

Semantic components
-------------------

Semantic components tie into the Odoo system and allow interaction with it.
remark: (with the remark style)
Placeholders are denoted in all caps.

.. _reference/user_interface/views/form/field:

<field>: render formatted values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <field name="FIELD_NAME"/>
  </form>

renders (and allow editing of, possibly) a single field of the current
record. Using several times a field in a form view is supported and the
fields can receive different values for modifiers 'invisible' and
'readonly'. This fields have the same values but can be display
differentaly. However, the behavior is not guaranteed when several
fields exist with different values for modifier 'required'. ``<field>``
can have the following attributes:

:name:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name

  the name of the field to render

:id:
  string_ (optional)

  the node id. Useful when there are several occurrences of the same field in
  the view (see ``label`` component below). Default is the field name.

:widget:
  string_ (optional)

  fields have a default rendering based on their type
  (e.g. :class:`~odoo.fields.Char`, :class:`~odoo.fields.Many2one`).

  The ``widget`` attributes allows using a different rendering method and context.
  See more information in :ref:`reference/js/widgets`

  .. code-block:: xml

    <field name="tag_ids" widget="many2many_tags"/>

:options:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a dict_ (default: ``{}``)

  JSON object specifying configuration option for the field's widget
  (including default widgets)

  .. code-block:: xml

    <field name="tag_ids" widget="many2many_tags" options="{'color_field': 'FIELD_NAME', 'no_quick_create': True}"/>

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  only displays the field for specific users

  .. code-block:: xml

    <field name="fname" groups="base.group_no_one,!base.group_multi_company"/>

:domain:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a :ref:`reference/orm/domains` (default: ``[]``)

  for relational fields only, filters to apply when displaying existing
  records for selection

  .. code-block:: xml

    <field name="fname_id" domain="[('fname_a', '=', parent.fname_b)]"/>

:context:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a dict_ (default: ``{}``)

  for relational fields only, context to pass when fetching possible values.
  The default values ``default_FIELD_NAME`` (e.g. ``{'default_name': 'toto'}``) will be used to
  create the linked record. ``OTHER_BUSINESS_KEY`` is every keys depending of the model/module.

  .. code-block:: xml

    <field name="fname" context="{
        'TYPE_view_ref': 'ADDON.MODEL_view_TYPE',
        'group_by': 'FIELD_NAME',
        'default_FIELD_NAME': ANY,
        'search_default_FIELD_NAME': True,
        'OTHER_BUSINESS_KEY': ANY,
      }"/>

:readonly:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  Whether the field can be modified by the user (``False``) or is read only (``True``).

  .. code-block:: xml

    <field name="fname_a" readonly="True"/>
    <field name="fname_b" readonly="name_a in [fname_b, parent.fname_d]"/>

:required:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  Whether the field can be left empty (``False``) or must be set (``True``).

  .. code-block:: xml

    <field name="fname_a" required="True"/>
    <field name="fname_b" required="fname_c != 3 and fname_a == parent.fname_d"/>

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  Whether the field can be visible (``False``) or must be hide (``True``).

  There are two uses for field ``invisible`` attribute:

  * Usability: not to overload the view and to make it easier for the user to read depending on the content;
  * Technical: fetched by the webclient for evaluating :ref:`python expression <user_interface/views/python_expression>`

  .. code-block:: xml

    <field name="fname_a" invisible="True"/> <!-- necessary to evaluate invisible attribute of 'fname_b' field -->
    <field name="fname_b" invisible="fname_c != 3 and fname_a == parent.fname_d"/>
    <field name="fname_c"/>

:nolabel:
  boolean_ (default: ``False``)

  if ``True``, do not automatically display the field's label, only
  makes sense if the field is a direct child of a ``group`` element

:placeholder:
  string_ (optional)

  help message to display in *empty* fields. Can replace field labels in
  complex forms. *Should not* be an example of data as users are liable to
  confuse placeholder text with filled fields

:mode:
  `Comma-separated values`_ (default: ``tree``) whose choices are: ``kanban``, ``from``, ``tree``

  for :class:`~odoo.fields.One2many`, display mode (view type) to use for
  the field's linked records. One of ``tree``, ``form``, ``kanban`` or
  ``graph``. The default is ``tree`` (a list display)

:help:
  string_ (optional)

  tooltip displayed for users when hovering the field or its label

:class:
  string_ (optional) `HTML class`_

  `HTML class`_ to set on the generated element.

  The styling use the Bootstrap_ framework and :ref:`UI icons <reference/user_interface/icons>`.

  Below are the common Odoo_ classes:

  ``oe_inline``: prevent the usual line break following fields and limit their span.

  ``oe_left``, ``oe_right``: floats_ the field to the corresponding direction

  ``oe_read_only``, ``oe_edit_only``: only displays the field in the corresponding form mode

  ``oe_avatar``: for image fields, displays images as "avatar" (square, 90x90 maximum size, some image decorations)

:filename:
  string_ (optional)

  for binary fields, name of the related field providing the name of the file

:password:
  boolean_ (default: ``False``)

  if is ``True`` indicates that a :class:`~odoo.fields.Char` field stores a
  password and that its data shouldn't be displayed

:kanban_view_ref:
  string_ (optional) defined by the pattern: ``%(ADDON.MODEL_view_TYPE)s`` (target the :ref:`view reference <reference/view_record>`)

  for opening specific kanban view when selecting records from m2o/m2m in mobile
  environment

:default_focus:
  boolean_ (default: ``False``)

  If True, defines that this field is the fields that will be focussed when the view
  opens. Cannot be present on more than one field of a view.

.. note::
  :ref:`Relational fields <studio/fields/relational-fields>` node can contain specific
  subviews.

  .. code-block:: xml

    <field name="children_ids">
      <tree>
        <field name="name"/>
      </tree>
      <form>
        <field name="id"/>
        <field name="name"/>
      </form>
    </field>

.. _reference/user_interface/views/form/label:

<label>: displays other field label
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

    <form>
      <div class="col col-md-auto">
        <label for="FIELD_NAME" string="LABEL"/>
        <div>
          <field name="FIELD_NAME" class="oe_inline"/>
        </div>
      </div>
    </form>


when a :ref:`field <reference/user_interface/views/form/field>` component
isn't placed directly inside a :ref:`group <reference/user_interface/views/form/group>`,
or when its ``nolabel`` attribute is set, the field's label isn't
automatically displayed alongside its value. The ``<label>`` component is the
manual alternative of displaying the label of a field. ``<label>`` can have the
following attributes:

:for:
  string_ (mandatory)

  the reference to the field associated with the label. Can be either the name
  of a field, or its id (``id`` attribute set on the
  :ref:`field <reference/user_interface/views/form/field>`). When there are several
  occurrences of the same field in the view, and there are several ``label``
  components associated with these :ref:`field <reference/user_interface/views/form/field>`
  nodes, those labels must have unique ``for`` attributes (in this case
  referencing the ``id`` attribute of the corresponding
  :ref:`field <reference/user_interface/views/form/field>` nodes).

:string:
  string_ (default: ``''``)

  the label to display. Display the field's label (coming from the field
  definition in the model) by default.

:class:
  string_ `HTML class`_ (default: ``''``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.


.. _reference/user_interface/views/form/button:

<button>: displays button to call action
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <button type="object" name="ACTION" string="LABEL"/>
    <button type="object" name="ACTION" icon="FONT_AWESOME"/>
  </form>

.. include:: views/include/component_button.rst

:class:
  string_ (optional) `HTML class`_

  In form view, in addition to the classic behavior, the special class
  ``oe_stat_button`` define a particular rendering in order to
  dynamically display information while being clickable to target an
  action.

  .. code-block:: xml

    <button type="object" name="ACTION" class="oe_stat_button" icon="FONT_AWESOME" help="HELP">
      <div class="o_field_widget o_stat_info">
        <span class="o_stat_value"><FIELD/></span>
        <span class="o_stat_text">TEXT</span>
      </div>
    </button>

:special:
  string_ (optional) chooses from ``save`` or ``cancel``

  for form views opened in dialogs: ``save`` to save the record and
  close the dialog, ``cancel`` to close the dialog without saving.

  .. code-block:: xml

    <button special="cancel" icon="fa-trash"/>

:confirm:
  string_ (optional)

  confirmation message to display (and for the user to accept) before
  performing the button's Odoo call (also works in Kanban views).

  .. code-block:: xml

    <button name="action_destroye_gate" string="Send the goa'uld" type="object" confirm="Do you confirm the action?"/>

:data-hotkey:
  string_ (optional) only one char or ``shift+`` + one char

  Define a hotkey (`keyboard_shortcut`_ similar to an accesskey_) enable when
  ``alt`` keypress to facilitate access to the action.

  .. code-block:: xml

    <button type="object" name="action_tear" string="Tear the sheet" data-hotkey="shift+k"/>

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

Messaging features
~~~~~~~~~~~~~~~~~~

Chatter is the communication and log tool located on most records. It
allows you to email colleagues and customers directly from a record
(task, order, invoice, event, note...).

The element must be a div with classname ``oe_chatter``.

The widget is linked to specific python code of this :ref:`reference/mixins/mail`.

.. code-block:: xml

    <form>
      <sheet>
        ...
      </sheet>
      <div class="oe_chatter">
        <field name="message_follower_ids"/>
        <field name="activity_ids"/>
        <field name="message_ids" options="OPTIONS"/>
      </div>
    </form>

Attachment/Document preview
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The element must be an empty div with classname ``o_attachment_preview``.

.. code-block:: xml

    <form>
      <sheet>
        ...
      </sheet>
      <div class="o_attachment_preview"/>
    <form>


.. _reference/user_interface/views/form/structural:

Structural components
---------------------

Structural components provide structure or "visual" features with little
logic. They are used as elements or sets of elements in form views.
Placeholders are denoted in all caps.

.. _reference/user_interface/views/form/group:

<group>: Columns layout
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <group>
      ...
    </group>
  </form>

Used to define column layouts in forms. By default, groups define 2 columns and most
direct children of groups take a single column.
:ref:`<field> <reference/user_interface/views/form/field>` direct children of groups
display a ``label`` by default, and the label and the field itself have a colspan of 1
each.

Children are laid out horizontally (tries to fill the next column before changing row).

``<group>`` can have the following attributes:

:col:
  integer_ (default: ``2``)

  number of columns in a ``<group>``

:colspan:
  integer_ (default: ``1``)

  number of columns taken by an element

:string:
  string_ (default: ``''``)

  displayed a group’s title

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <group>
      <field name="a" string="custom"/>
      <field name="b"/>
    </group>
    <group string="title 1">
      <group string="title 2">
        <field name="c"/>
        <field name="d"/>
      </group>
      <group>
        <field name="e"/>
        <field name="f"/>
        <field name="g"/>
      </group>
    </group>
    <group col="12">
      <group colspan="8">
        <field name="h"/>
      </group>
      <group colspan="4">
        <field name="i"/>
      </group>
    </group>

  .. image:: views/form_group.svg
    :class: col-xxl-6

.. _reference/user_interface/views/form/sheet:

<sheet>: Responsive layout
~~~~~~~~~~~~~~~~~~~~~~~~~~

``<sheet>`` can be used as a direct child to ``<form>`` for a narrower and more responsive
form layout (center page, margin...). Usually it contains :ref:`<group> <reference/user_interface/views/form/group>`.

.. code-block:: xml

  <form>
    <sheet>
      ...
    </sheet>
  </form>

.. _reference/user_interface/views/form/notebook:

<notebook> & <page>: tabbed section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <notebook>
      <page string="LABEL">
        ...
      </page>
    </notebook>
  </form>

``notebook`` defines a tabbed section. Each tab is defined through a ``page``
child element.

``<page>`` can have the following attributes:

:string:
  string_ (default: ``''``)

  the title of the tab

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

  Can be apply on ``notebook`` and ``page`` nodes.

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <form>
      <notebook>
        <page string="Page1">
          ...
        </page>
        <page string="Page2">
          ...
        </page>
      </notebook>
    </form>

  .. image:: views/form_notebook.svg
    :class: col-xxl-6

.. note:: Note that ``notebook`` should not be placed within ``group``

.. _reference/user_interface/views/form/newline:

<newline>: new row
~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <group>
      ...
      <newline/>
      ...
    </group>
  </form>

only useful within :ref:`<group> <reference/user_interface/views/form/group>`
elements, ends the current row early and immediately switches to a new row
(without filling any remaining column beforehand)

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <form>
      <group string="Title 1">
        <group string="Title 1.1">...</group>
        <newline/>
        <group string="Title 1.2">...</group>
        <group string="Title 1.3">...</group>
      </group>
    </form>

  .. image:: views/form_newline.svg
    :class: col-xxl-6

.. _reference/user_interface/views/form/separator:

<separator>: horizontal spacing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    ...
    <separator/>
    ...
  </form>

small horizontal spacing. ``<separator>`` can have the following attributes:

:string:
  string_ (default: ``''``)

  the title as a section title

Below is a possible structure and the representation of its rendering.

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <form>
      <group>
        <FIELD/>
        <separator string="Title 1"/>
        <FIELD/>
        <group>
          <FIELD/>
          <separator string="Title 2"/>
          <FIELD/>
        </group>
        <group>
          <FIELD/>
          <FIELD/>
        </group>
      </group>
    </form>

  .. image:: views/form_separator.svg
    :class: col-xxl-6

.. _reference/user_interface/views/form/header:

<header>: workflow buttons and status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <header>
      <BUTTONS/>
    </header>
    <sheet>
      ...
    </sheet>
  </form>

combined with :ref:`<sheet> <reference/user_interface/views/form/sheet>`,
provides a full-width location above the sheet itself, generally used to
display workflow :ref:`buttons <reference/user_interface/views/form/button>`
and a :ref:`field <reference/user_interface/views/form/field>` display as
status widget.

Below is an example with the status widget with some options.

.. code-block:: xml

  <header>
    <button string="Reset" type="object" name="set_draft" invisible="state != 'done'"/>
    <field name="state" widget="statusbar" statusbar_visible="draft,posted" options="{'clickable': 1}"/>
  </header>

.. _reference/user_interface/views/form/footer:

<footer>: bottom/dialog buttons
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

  <form>
    <sheet>
      ...
    </sheet>
    <footer>
      <BUTTONS/>
    </footer>
  </form>

Display button at the bottom of the dialog. Used with :ref:`buttons <reference/user_interface/views/form/button>`

The special action from ``<button>`` can save or cancel the form view displayed into the
dialog.

.. code-block:: xml

  <footer>
      <button string="Save" special="save"/>
      <button string="Feature action" type="object" name="my_action" class="btn-primary"/>
      <button string="Discard" special="cancel"/>
  </footer>

.. _reference/user_interface/views/form/container:

container for buttons
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

    <form>
      <div name="button_box">
        <BUTTONS/>
      </div>
    <form>

Container for specific rendering to display :ref:`buttons <reference/user_interface/views/form/button>`

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <form>
      <div name="button_box">
        <button type="edit" name="edit"
            icon="fa-edit" string="Button1"/>

        <button type="object" name="my_action"
            icon="fa-dollar">
          <field name="total_inv" widget="statinfo"
              string="Invoices"/>
        </button>
      </div>
      ...
    <form>

  .. image:: views/form_button_box.svg
    :class: col-xxl-6

container for a title
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

    <form>
      <sheet>
        <div class="oe_title">
          <h1><FIELD/></h1>
        </div>
      </sheet>
    <form>

Container for specific rendering to display a :ref:`<field> <reference/user_interface/views/form/field>` as title.


.. _`keyboard_shortcut`: https://en.wikipedia.org/wiki/Keyboard_shortcut
.. _Odoo: https://www.odoo.com/
.. _Bootstrap: https://getbootstrap.com/
.. _HTML: https://en.wikipedia.org/wiki/HTML
.. _attributes: https://en.wikipedia.org/wiki/HTML_attribute
.. _`HTML class`: https://en.wikipedia.org/wiki/HTML_attribute
.. _accesskey: https://www.w3.org/TR/html5/editing.html#the-accesskey-attribute
.. _floats: https://developer.mozilla.org/en-US/docs/Web/CSS/float
.. _`relative path`: https://en.wikipedia.org/wiki/URL
.. _path: https://en.wikipedia.org/wiki/Path_(computing)
.. _`Comma-separated values`: https://en.wikipedia.org/wiki/Comma-separated_values
.. _integer: https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _dict: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
