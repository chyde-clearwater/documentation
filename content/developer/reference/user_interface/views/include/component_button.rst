``<button>`` can have the following attributes:

:icon:
  string_ (optional)

  icon to use to display the button (:ref:`UI icons <reference/user_interface/icons>`)

  .. code-block:: xml

    <button type="object" name="remove" icon="fa-trash"/>

:string:
  string_ (default: ``''``)

  * if there is no ``icon``, the button's text
  * if there is an ``icon``, ``alt`` text for the icon

  .. code-block:: xml

      <button type="object" name="action_create_new" string="Create document"/>

:help:
  string_ (optional)

  add a tooltip message when hover with the mouse cursor

  .. code-block:: xml

    <button type="object" name="remove" icon="fa-trash" help="Revoke"/>

:context:
  `python expression`_ that evaluates to a dict_ (default: ``{}``)

  merged into the view's context when performing the button's Odoo call

  .. code-block:: xml

    <button name="button_confirm" type="object" context="{'BUSINESS_KEY': ANY}" string="LABEL"/>

:type:
  string_ chooses from ``object`` or ``action`` (mandatory)

  type of button, indicates how it clicking it affects Odoo:

  .. rst-class:: o-definition-list

  ``object``
      call a method on the list's model. The button's ``name`` is the
      method, which is called with the current row's record id and the
      current context.

      .. web client also supports a @args, which allows providing
          additional arguments as JSON. Should that be documented? Does
          not seem to be used anywhere

  ``action``
      load an execute an ``ir.actions``, the button's ``name`` is the
      database id of the action. The context is expanded with the list's
      model (as ``active_model``), the current row's record
      (``active_id``) and all the records currently loaded in the list
      (``active_ids``, may be just a subset of the database records
      matching the current search)

  .. code-block:: xml

      <button type="object" name="action_create_new" string="Create document"/>
      <button type="action" name="%(addon.action_create_view)d" string="Create and Edit"/>

:name:
  string_ (optional)

  see ``type``

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  same as for :ref:`form field <reference/user_interface/views/form/field>` component.

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  same as for :ref:`form field <reference/user_interface/views/form/field>` component.

:class:
  string_ (optional) `HTML class`_

  same as for :ref:`form field <reference/user_interface/views/form/field>` component.


.. _`python expression`: https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _dict: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
.. _`HTML class`: https://en.wikipedia.org/wiki/HTML_attribute
