.. _reference/user_interface/views/kanban:

Kanban
======

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <kanban>
      ...
    </kanban>

  .. image:: views/kanban.svg
    :class: col-xxl-6

The kanban view is a `kanban board`_ visualisation: it displays records as
"cards", halfway between a :ref:`list view <reference/user_interface/views/list>` and a
non-editable :ref:`form view <reference/user_interface/views/form>`. Records may be grouped
in columns for use in workflow visualisation or manipulation (e.g. tasks or
work-progress management), or ungrouped (used simply to visualize records).

.. note:: The kanban view will load and display a maximum of ten columns.
          Any column after that will be closed (but can still be opened by
          the user).

The root element of the Kanban view is ``<kanban>``, it can use the following
attributes_:

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

:delete:
  boolean_ (default: ``True``)

  Disable/enable record deletion on the view through the **Action** dropdown.

:default_group_by:
  string_ (optional) :ref:`model <reference/orm/model>` field name

  whether the kanban view should be grouped if no grouping is specified via
  the action or the current search. Should be the name of the field to group
  by when no grouping is otherwise specified

:default_order:
  string_ (optional)

  cards sorting order used if the user has not already sorted the records (via
  the list view)

:class:
  string_ (optional) `HTML class`_

  adds HTML classes to the root HTML element of the Kanban view

:examples:
  string_ (optional)

  if set to a key in the `KanbanExamplesRegistry`_, examples on column setups will be available in the grouped kanban view. `Here <https://github.com/odoo/odoo/blob/99821fdcf89aa66ac9561a972c6823135ebf65c0/addons/project/static/src/js/project_task_kanban_examples.js#L27>`_ is an example of how to define those setups.

:group_create:
  boolean_ (default: ``True``)

  whether the "Add a new column" bar is visible or not.

:group_delete:
  boolean_ (default: ``True``)

  whether groups can be deleted via the context menu.

:group_edit:
  boolean_ (default: ``True``)

  whether groups can be edited via the context menu.

:archivable:
  boolean_ (default: ``True``)

  whether records belonging to a column can be archived / restored if an
  ``active`` field is defined on the model.

:quick_create:
  boolean_ (default: ``True``)

  whether it should be possible to create records without switching to the
  form view. By default, ``quick_create`` is enabled when the Kanban view is
  grouped by many2one, selection, char or boolean fields, and disabled when not.

:quick_create_view:
  string_ (optional)

  :ref:`Form <reference/user_interface/views/form>` view reference, specifying the view used for records quick creation.

:records_draggable:
  boolean_ (default: ``True``)

  whether it should be possible to drag records when kanban is grouped.

  Set to ``true`` to always enable it, and to ``false`` to always disable it.

:groups_draggable:
  boolean_ (default: ``True``)

  whether it should be possible to resequence colunms when kanban is grouped.

  Set to ``true`` to always enable it, and to ``false`` to always disable it.

.. todo:: VFE missing information on on_create attribute of kanban views.

Possible children elements of the kanban view are: ``field``, ``progressbar`` or ``templates``

.. _reference/user_interface/views/kanban/field:

<field>: render formatted values
--------------------------------

.. code-block:: xml

  <kanban>
    <field name="FIELD_NAME"/>
    ...
  </kanban>

declares fields to use in kanban *logic*. If the field is simply displayed in
the kanban view, it does not need to be pre-declared.

Fields can use the following attributes:

:name:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name

  the name of the field to fetch

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <kanban>
      <templates>
        <t t-name="kanban-box">
          <div>
            <field name="name"/>
          </div>
        </t>
      </templates>
    </kanban>

  .. image:: views/kanban_field.svg
    :class: col-xxl-6

.. _reference/user_interface/views/kanban/header:

<header>: custom buttons in control panel
-----------------------------------------

.. code-block:: xml

  <kanban>
    <header>
      <BUTTONS/>
    </header>
    ...
  </kanban>

.. include:: views/include/header_buttons.rst

.. note::

  Currently, only the ``always`` option is usable because it is not yet possible
  to select records in a kanban view. This should happen soon.

.. _reference/user_interface/views/kanban/progressbar:

<progressbar>: progressbar on top of columns
--------------------------------------------

.. code-block:: xml

  <kanban>
    <progressbar field="FIELD_NAME"/>
    ...
  </kanban>

declares a progressbar element to put on top of kanban columns.

Possible attributes are:

:field:
  string_ (mandatory) :ref:`model <reference/orm/model>` field name

  the name of the field whose values are used to subgroup column's records in
  the progressbar

:colors:
  JSON_ (mandatory)

  JSON mapping the above field values to either "danger", "warning", "success"
  or "muted" colors

:sum_field:
  string_ (optional) :ref:`model <reference/orm/model>` field name

  the name of the field whose column's records' values will be summed and
  displayed next to the progressbar (if omitted, displays the total number of
  records)

.. container:: row

  .. code-block:: xml
    :class: col-xxl-6

    <kanban>
      <progressbar field="activity_state"
          colors="{'planned': 'success', 'today': 'warning', 'overdue': 'danger'}"
          sum_field="expected_revenue"/>
      <templates>
        ...
      </templates>
    </kanban>

  .. image:: views/kanban_progressbar.svg
    :class: col-xxl-6

.. _reference/user_interface/views/kanban/templates:

<templates>: template of card
-----------------------------

.. code-block:: xml

  <kanban>
    ...
    <templates>
      <t t-name="kanban-box">
        <div>
          <field name="name"/>
        </div>
      </t>
    </templates>
  </kanban>

defines a list of :ref:`reference/qweb` templates. Cards definition may be
split into multiple templates for clarity, but kanban views *must* define at
least one root template ``kanban-box``, which will be rendered once for each
record.

Two additional templates can be defined: ``kanban-menu`` and ``kanban-tooltip``.
If defined, the ``kanban-menu`` template is rendered inside a dropdown that can be
toggled with a vertical ellipsis (:guilabel:`⋮`) on the top right of the card.
The ``kanban-tooltip`` template is rendered inside a tooltip when hovering kanban cards.

The kanban view uses mostly-standard :ref:`javascript qweb
<reference/qweb/javascript>` and provides the following context variables:

.. rst-class:: o-definition-list

``widget``
  the current :js:class:`KanbanRecord`, can be used to fetch some
  meta-information. These methods are also available directly in the
  template context and don't need to be accessed via ``widget``
``record``
  an object with all the requested fields as its attributes. Each field has
  two attributes ``value`` and ``raw_value``, the former is formatted
  according to current user parameters, the latter is the direct value from
  a :meth:`~odoo.models.Model.read` (except for date and datetime fields
  that are `formatted according to user's locale
  <https://github.com/odoo/odoo/blob/a678bd4e/addons/web_kanban/static/src/js/kanban_record.js#L102>`_)
``context``
  the current context, coming from the action, and the one2many or many2many
  field in the case of a Kanban view embedded in a Form view
``user_context``
  self-explanatory
``read_only_mode``
  self-explanatory
``selection_mode``
  set to true when kanban view is opened in mobile environment from m2o/m2m field
  for selecting records.

  .. note:: clicking on m2o/m2m field in mobile environment opens kanban view


  .. rubric:: buttons and fields

  While most of the Kanban templates are standard :ref:`reference/qweb`, the
  Kanban view processes ``field``, ``button`` and ``a`` elements specially:

  * by default fields are replaced by their formatted value, unless the
    ``widget`` attribute is specified, in which case their rendering and
    behavior depends on the corresponding widget. Possible values are (among
    others):

    .. rst-class:: o-definition-list

    ``handle``
        for ``sequence`` (or ``integer``) fields by which records are
        sorted, allows to drag&drop records to reorder them.

    .. todo:: list widgets?

  * buttons and links with a ``type`` attribute become perform Odoo-related
    operations rather than their standard HTML function. Possible types are:

    .. rst-class:: o-definition-list

    ``action``, ``object``
      standard behavior for :ref:`Odoo buttons
      <reference/user_interface/views/list/button>`, most attributes relevant to standard
      Odoo buttons can be used.
    ``open``
      opens the card's record in the form view in read-only mode
    ``edit``
      opens the card's record in the form view in editable mode
    ``delete``
      deletes the card's record and removes the card

  .. todo::

      * kanban-specific CSS
      * kanban structures/widgets (vignette, details, ...)

If you need to extend the Kanban view, see :js:class:`KanbanRecord`.


.. _kanban board: https://en.wikipedia.org/wiki/Kanban_board
.. _KanbanExamplesRegistry: https://github.com/odoo/odoo/blob/99821fdcf89aa66ac9561a972c6823135ebf65c0/addons/web/static/src/js/views/kanban/kanban_examples_registry.js
.. _attributes: https://en.wikipedia.org/wiki/HTML_attribute
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
.. _`HTML class`: https://en.wikipedia.org/wiki/HTML_attribute
.. _JSON: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
